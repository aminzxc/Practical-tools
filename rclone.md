### install rclone
```
curl https://rclone.org/install.sh | sudo bash
```
### connect to Google Drive
```
rclone config
```
### Creating an Access Token on Windows
```
download rclone window with url https://rclone.org/downloads/
cd C:\Users\amin\Downloads\rclone-v1.70.2-windows-amd64\rclone-v1.70.2-windows-amd64 
rclone authorize "drive" "eyJzY29wZSrfI6ImRyaXZlIn0"
```
### Copy from Server To Drive
```
rclone copy backup-oto backup-oto:BackupServer-oto --progress
```
### command
```
rclone listremotes

```
