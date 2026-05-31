import urllib.request

urls = [
    "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/refs/heads/main/Vless-Reality-White-Lists-Rus-Mobile-2.txt",
    "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/refs/heads/main/Vless-Reality-White-Lists-Rus-Mobile.txt",
    "https://raw.githubusercontent.com/igareck/vpn-configs-for-russia/refs/heads/main/BLACK_VLESS_RUS.txt"
]

headers = (
    "# profile-title: XpaVPN | Белые Списки\n"
    "# profile-update-interval: 1\n"
    "# profile-web-page-url: https://t.me/XpaVPN\n"
    "# support-url: https://t.me/XpaneR\n"
    "# announce: Поддержка: @XpaneR | 🔄 Обновление каждые 2 часа\n"
    "# subscription-userinfo: upload=0;download=1099511627776000;total=0;expire=2524608000"
)

unique_configs = set()

for url in urls:
    try:
        req = urllib.request.Request(url, headers={"User-Agent": "Mozilla/5.0"})
        with urllib.request.urlopen(req) as response:
            content = response.read().decode("utf-8")
            for line in content.splitlines():
                cleaned = line.strip()
                if cleaned and not cleaned.startswith("#"):
                    unique_configs.add(cleaned)
    except Exception as e:
        print(f"Ошибка при скачивании {url}: {e}")

final_content = headers + "\n" + "\n".join(sorted(unique_configs))

with open("index.html", "w", encoding="utf-8") as f:
    f.write(final_content)

print("Файл index.html успешно обновлен!")
