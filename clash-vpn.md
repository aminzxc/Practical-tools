### install clash
```
https://github.com/MetaCubeX/mihomo/releases/download/v1.19.11/mihomo-linux-amd64-v1.19.11.gz
```
### enable tun
```
sudo modprobe tun
ls /dev/net/tun
```
### extract file
```
gzip -d mihomo-linux-amd64-v1.19.11.gz
mv mihomo-linux-amd64-v1.19.11 mihomo
chmod +x mihomo
```
### Transfer to the executive path
```
sudo mv mihomo /usr/local/bin/clash-meta
```
### check version
```
clash-meta -v
```
### make config file
```
mkdir -p ~/.config/clash
sudo nano /root/.config/clash/config.yaml
```
### config
```
port: 7890
socks-port: 7891
redir-port: 7892
allow-lan: true
mode: rule
log-level: info

proxies:
  - name: offi-ubuntu-server
    type: vless
    server: 185.110.190.148
    port: 443
    uuid: 8d66c5b8-bace-49d8-bf97-ff20ca35d39d
    flow: xtls-rprx-vision
    udp: true
    network: tcp
    tls: true
    client-fingerprint: chrome
    servername: www.google.com
    reality-opts:
      public-key: -JHVAx0CH9Qa84OvTc9ISwPHsEMe7_4MhoPW6bP9GkU
      short-id: 9d
      spider-x: /

proxy-groups:
  - name: Proxy
    type: select
    proxies:
      - offi-ubuntu-server

rules:
  - MATCH,Proxy

tun:
  enable: true
  stack: system
  auto-route: true
  auto-detect-interface: true

dns:
  enable: true
  listen: 0.0.0.0:53
  ipv6: false
  nameserver:
    - 8.8.8.8
    - 1.1.1.1
  default-nameserver:
    - 8.8.8.8
    - 1.1.1.1
  fallback:
    - 1.0.0.1
    - 8.8.4.4
```
### run vpn
```
sudo clash-meta -d /root/.config/clash
```
### test connection
```
curl -x socks5h://127.0.0.1:7891 https://ifconfig.me
```
### service clash
```
vim /etc/systemd/system/clash-meta.service
```
```
[Unit]
Description=Clash.Meta (Mihomo) - TUN Proxy Service
After=network.target nss-lookup.target

[Service]
Type=simple
ExecStart=/usr/local/bin/clash-meta -d /root/.config/clash
Restart=on-failure
RestartSec=5
LimitNOFILE=1048576
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target

or

[Unit]
Description=Clash.Meta Daemon
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/clash-meta -d /root/.config/clash
Restart=on-failure

[Install]
WantedBy=multi-user.target

```
```
sudo systemctl daemon-reexec
sudo systemctl enable clash-meta
sudo systemctl start clash-meta
```
### log & status
```
journalctl -u clash-meta -f
```
### convert config v2ray to clash
```
https://github.com/tindy2013/subconverter
https://acl4ssr.netlify.app
```
