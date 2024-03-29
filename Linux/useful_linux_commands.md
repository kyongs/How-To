### Useful Linux Commands

1. tar.gz zip unzip
```bash
# zip
$ tar -zcvf [folder_name.tar.gz] [folder_name]

# unzip
$ tar -zxvf [folder_name.tar.gz]
```

2. scp
```bash
# local -> remote
$ scp [file_name] [remote_user_name]@[remote_ip]:[location]
# ex) scp testfile root@192.168.159.129:/tmp/testclient

# remote -> local
$ scp [remote_user_name]@[remote_ip]:[location] [local_location]
# ex) scp root@192.168.159.129:tmp/testclient/testfile /tmp
```

3. shutdown
```bash
$ sudo shutdown -h now
```

4. How to change the "\n" in a text file to a new line in Linux

```bash
$ sed 's/\\n/\n/g' [file name]
# ex) sed 's/\\n/\n/g' innodb-status.txt
```

5. How to extract the number of occurrences of a specific string within a file.
```

$ cat office.txt

word
excel
powerpoint
word
word

$ grep -o "word" office.txt | wc -w
3
```
