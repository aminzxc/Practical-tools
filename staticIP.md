### config netplan
```
network:
  version: 2
  ethernets:
    ens33:
      macaddress: 02:4f:9d:b2:18:cd
      dhcp4: no
      addresses:
        - 192.168.228.131/24
      routes:
        - to: 0.0.0.0/0
          via: 192.168.228.2
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```
### check gateway
```
route -n
```
### generate MAC address
```
printf '02:%02x:%02x:%02x:%02x:%02x\n' $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256))
```
