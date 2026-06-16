### install proxy on server
```
curl -o MTProtoProxyInstall.sh -L https://git.io/fjo34 && bash MTProtoProxyInstall.sh
ref: https://github.com/HirbodBehnam/MTProtoProxyInstaller
```
### tunnel from iran to proxy
```
bash <(curl -Ls --ipv4 https://github.com/Musixal/haproxy/raw/main/haproxy.sh)
ref: https://github.com/Musixal/HAProxy
```
### youtube
```
https://www.youtube.com/watch?v=Z5uHg4H1GS0
```
### proxy with service docker
```
vim config.toml

secret = "ee5ac726a1fbeb55c6f05cfaebd4c2ad3f636c6f7564666c6172652e636f6d"

bind-to = "0.0.0.0:3128"
concurrency = 8192
prefer-ip = "prefer-ipv4"
public-ipv4 = "185.110.191.11"

[network]
dns = "https://1.1.1.1"

[defense.anti-replay]
enabled = true
max-size = "1mib"

[stats.prometheus]
enabled = true
bind-to = "0.0.0.0:3129"
http-path = "/"
metric-prefix = "mtg"
```
```
vim docker-compose.yml

services:
  mtg-proxy:
    image: nineseconds/mtg:2
    container_name: m2dm
    restart: unless-stopped

    ports:
      - "4443:3128/tcp"
      - "127.0.0.1:3129:3129/tcp"

    volumes:
      - ./config.toml:/config.toml:ro

    ulimits:
      nofile:
        soft: 65536
        hard: 65536

    logging:
      driver: json-file
      options:
        max-size: "20m"
        max-file: "5"

```
### test config
```
docker run --rm \
  -v "$PWD/config.toml:/config.toml:ro" \
  nineseconds/mtg:2 doctor /config.toml
```
### generate link
```
docker exec mtg-proxy /mtg access /config.toml
```
### generate secret for fake TLS
```
cd /opt/telegram-mtproxy

FAKE_TLS_DOMAIN="tg.30bime.ir"

SECRET=$(docker run --rm nineseconds/mtg:2 generate-secret --hex "$FAKE_TLS_DOMAIN")

echo "$SECRET"
```
### monitoring with Prometheus
```
scrape_configs:
  - job_name: "mtg-proxy"
    static_configs:
      - targets: ["127.0.0.1:3129"]
```
### show metric
```
curl http://127.0.0.1:3129/
```
