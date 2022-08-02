## How to Run MySQL After Modifying Source Code

#### 1. Rebuild MySQL
Make sure MySQL is completely shutdown!

```Shell
# shutdown if needed
$ cd /usr/local/mysql
$ ./bin/mysqladmin -uroot -p1234 shutdown


# build source code
$ cd ~/mysql-8.0.28/bld
$ sudo cmake ..
$ sudo make -j16 install
```

#### 2. Restart MySQL

```Shell
$ cd /usr/local/mysql
$  ./bin/mysqld_safe --defaults-file=/usr/local/mysql/my.cnf
```

#### 3. Empty the Log file if needed.
```Shell
$ cd ~/log-data
$ cat /dev/null > mysql_error.log
```

#### 4. Run the benchmark (In this case, TPC-C)
```Shell
$ cd ~/tpcc-mysql
$ ./tpcc_start -h 127.0.0.1 -S /tmp/mysql.sock -d tpcc -u root -p1234 -w 100 -c 8 -r 10 -l 600 | tee tpcc-result.txt

```
