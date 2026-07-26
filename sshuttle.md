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
 149.154.166.110:443
```
