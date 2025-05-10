### install sshuttle on server
```
apt install sshuttle
```
### connect sshuttle
```
sshuttle -vvv -r user@ip:port  0.0.0.0/0 --exclude local-ip --dns
```
### The destination server user must have shell
### It is best used for use within tmux or screen
