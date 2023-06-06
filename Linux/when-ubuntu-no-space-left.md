#### system log 지우는법

(rm 으로 지우면 안됨)

```bash
$ sudo sh -c 'cat /dev/null > /var/log/syslog'
$ cat /dev/null > /var/log/kern.log'
```
