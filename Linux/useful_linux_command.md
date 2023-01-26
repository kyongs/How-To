### Useful Linux Command

1. tar.gz zip unzip
```bash
# zip
tar -zcvf [folder_name.tar.gz] [folder_name]

# unzip
tar -zxvf [folder_name.tar.gz]
```

2. scp
```bash
# local -> remote
scp [file_name] [remote_user_name]@[remote_ip]:[location]
# ex) scp testfile root@192.168.159.129:/tmp/testclient

# remote -> local
scp [remote_user_name]@[remote_ip]:[location] [local_location]
# ex) scp root@192.168.159.129:tmp/testclient/testfile /tmp
```

3. shutdown
```bash
sudo shutdown -h now
```
