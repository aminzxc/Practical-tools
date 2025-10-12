### install open vpn
```
wget https://git.io/vpn -O openvpn-install.sh
chmod +x openvpn-install.sh
./openvpn-install.sh
```
### config open vpn
```
# vim /etc/openvpn/server/server.conf
local PUBLIC IP
port 1194
proto udp
dev tun
ca ca.crt
cert server.crt
key server.key
dh dh.pem
auth SHA512
tls-crypt tc.key
topology subnet
server 10.8.0.0 255.255.255.0
server-ipv6 fddd:1194:1194:1194::/64
push "redirect-gateway def1 ipv6 bypass-dhcp"
ifconfig-pool-persist ipp.txt
#push "dhcp-option DNS 1.1.1.1"
#push "dhcp-option DNS 1.0.0.1"
push "block-outside-dns"
keepalive 10 120
user nobody
group nogroup
persist-key
persist-tun
verb 3
crl-verify crl.pem
explicit-exit-notify
push "dhcp-option DNS 10.8.0.1"
push "dhcp-option DOMAIN vpn"

```
### install dnsmasq
```
apt install -y dnsmasq
```
### config dnsmasq
```
# vim /etc/dnsmasq.conf
domain=vpn
expand-hosts
bind-interfaces
address=/nginx.vpn/10.8.0.1
address=/grafana.vpn/10.8.0.1
address=/api.vpn/10.8.0.1
listen-address=127.0.0.1,10.8.0.1
server=1.1.1.1
server=1.0.0.1
```
### restart dnsmasq
```
systemctl restart dnsmasq
```
### test
```
dig @10.8.0.1 nginx.vpn
```
### access to service with vpn
```
# config nginx
server {
    listen 10.8.0.1:80;
    server_name grafana.vpn;

    proxy_set_header Host              $host;
    proxy_set_header X-Real-IP         $remote_addr;
    proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_http_version 1.1;
    proxy_set_header Connection "";

    location / {
        proxy_pass http://10.8.0.1:8077;
    }
}

server {
    listen 10.8.0.1:80;
    server_name api.vpn;

    proxy_set_header Host              $host;
    proxy_set_header X-Real-IP         $remote_addr;
    proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_http_version 1.1;
    proxy_set_header Connection "";

    location / {
        proxy_pass http://10.8.0.1:8088;
    }
}

```
### config docker service
```
docker run -it -p 10.8.0.1:8077:80 nginx
```
### multi domain without nginx
```
http://grafana.vpn:8083
```
