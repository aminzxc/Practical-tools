### install sshuttle on server
```
apt install sshuttle
```
### connect sshuttle for app host linux
```
sshuttle -vvv -r user@ip:port  0.0.0.0/0 --exclude local-ip  --exclude server-ip --dns
```
### The destination server user must have shell
### It is best used for use within tmux or screen
### access docker to sshuttle
```
sshuttle -vvv \
  --method nat \
  --listen 0.0.0.0:12300 \
  -r root@82.115.19.61:22 \
  --exclude 82.115.19.61/32 \
  --exclude 172.24.11.199/32 \
  --exclude 172.18.0.0/16 \  # range docker network
  --dns \
  --disable-ipv6 \
  0.0.0.0/0
```
### Telegram traffic only.
```
sshuttle -vvv \
 --method nat \
 --listen 0.0.0.0:12300 \
 -r root@82.115.19.61:22 \
 --exclude 82.115.19.61/32 \
 --disable-ipv6 \
 149.154.166.110:443  # api.telegram.org
```
```
getent ahostsv4 api.telegram.org
```
### sshuttle as a service
```
vim /etc/systemd/system/sshuttle-telegram.service

[Unit]
Description=Persistent sshuttle tunnel for Telegram API
Documentation=https://sshuttle.readthedocs.io/
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=0

[Service]
Type=simple
User=root

ExecStart=/usr/bin/sshuttle -v \
    --method nat \
    --listen 0.0.0.0:12300 \
    --remote root@82.115.19.61:22 \
    --exclude 82.115.19.61/32 \
    --disable-ipv6 \
    --ssh-cmd "/usr/bin/ssh -i /root/.ssh/id_ed25519 -o BatchMode=yes -o StrictHostKeyChecking=yes -o ConnectTimeout=10 -o ConnectionAttempts=1 -o ServerAliveInterval=15 -o ServerAliveCountMax=3 -o TCPKeepAlive=yes" \
    149.154.166.110/32:443

Restart=always
RestartSec=10s
TimeoutStartSec=60s
TimeoutStopSec=30s

[Install]
WantedBy=multi-user.target
```
```
systemctl daemon-reload
systemctl enable --now sshuttle-telegram.service
```
### test with docker container
```
docker run --rm \
                         --network container:zabbix-server \
                         curlimages/curl:8.12.1 \
                         -sS -o /dev/null \
                         --connect-timeout 10 \
                         --max-time 20 \
                         -w 'IP=%{remote_ip} HTTP=%{http_code} Connect=%{time_connect}s TLS=%{time_appconnect}s Total=%{time_total}s\n' \
                         https://api.telegram.org/
```
