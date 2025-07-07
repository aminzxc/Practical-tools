### config DOH
```
https://github.com/Ptechgithub/smartSNI#
youtube: https://www.youtube.com/watch?v=D6IM5Nk_Bng
```
### config on server
```
systemctl disable systemd-resolved --now
docker run -d -p 53:53/udp -p 443:443 -p 80:80 --net=host -e PUB_IP=YOUR_PUBLIC_IP -e DNS_ALLOW_ALL=YES --name some-byosh mosajjal/byosh:latest
```
### test other server
```
 nslookup google.com 3.88.158.132
Server:         3.88.158.132
Address:        3.88.158.132#53

Non-authoritative answer:
Name:   google.com
Address: 216.239.38.120
Name:   google.com
Address: 2001:4860:4802:32::78

oto@master-1 ~> dig @3.88.158.132 google.com

; <<>> DiG 9.18.30-0ubuntu0.24.04.2-Ubuntu <<>> @3.88.158.132 google.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3379
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             418     IN      A       216.239.38.120

;; Query time: 16 msec
;; SERVER: 3.88.158.132#53(3.88.158.142) (UDP)
;; WHEN: Mon Jun 30 10:22:10 +0330 2025
;; MSG SIZE  rcvd: 44
```
### Set up on server
```
vim /etc/resolv.conf
nameserver 3.88.158.132
```
