## Build and Install the MySQL source code (5.7)

This document is based on Ubuntu 18.04, MySQL 5.7.33
<br/>

### Prerequisites

---

```bash
$ sudo apt-get install libreadline7
$ sudo apt-get install libaio1 libaio-dev
$ sudo apt-get install build-essential cmake libncurses5 libncurses5-dev bison
$ sudo apt-get install gcc-8 g++-8
$ sudo apt-get install libssl-dev
$ sudo apt-get install pkg-config
```

### Build and Install

---

1. Download the source code from [MySQL download URL.]([https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)) (**Select OS**: Source Code, **Select OS Version**: All Operating Systems)

```bash
$ wget https://dev.mysql.com/get/Downloads/MySQL-/mysql-5.7.33.tar.gz
```

<br/>

3. Unzip the `tar.gz` file and make the build file inside the source code. Follow the error message if the boost file does not exist in your MySQL directory. <br/> You would have to add `-DWITH` option and `-DDOWNLOAD` option with the `cmake` operation. I executed the `make` command by allocating 16 CPUs by `-j16` option. This is optional. (It will take much time if you get rid of this option, though.)

```bash
$ tar zxvf mysql-5.7.33.tar.gz
$ cd mysql-5.7.33

# 1) if you are installing at first time
$ cmake -DDOWNLOAD_BOOST=ON -DWITH_BOOST=/path/to/download/boost -DCMAKE_INSTALL_PREFIX=/path/to/dir
# cmake -DDOWNLOAD_BOOST=ON -DWITH_BOOST=/home/vldb/mysql-5.7.33 -DCMAKE_INSTALL_PREFIX=/usr/local/mysql

# 2) if you already have a boost library
$ cmake -DWITH_BOOST=/path/to/boost -DCMAKE_INSTALL_PREFIX=/path/to/dir

$ sudo make -j16 install
```

<br/>

4. MySQL makes the base directory in `/usr/local/mysql` by default. In this document, I will make the data directory(`test-data`) at the `home` directory.Initialize MySQL with the following command.

```shell
$ cd /usr/local/mysql
$ ./bin/mysqld --initialize --user=mysql --datadir=/path/to/datadir --basedir=/path/to/basedir
# $ ./bin/mysqld --initialize --user=mysql --datadir=/home/vldb/test_data --basedir=/usr/local/mysql
```

<br/>

5. Set password. Run the MySQL Server with `--skip-grant-tables` command. You can check with `ps -ef | grep mysql` command if MySQL server is operating.

```bash
$ ./bin/mysqld_safe --skip-grant-tables --datadir=/path/to/datadir
```

on the other terminal window or tab (location: /usr/local/mysql)

```bash
# check mysql if it is running correctly
$ ps -ef | grep mysql

$ cd /usr/local/mysql
$ ./bin/mysql -uroot

mysql> use mysql;
mysql> update user set authentication_string=password('yourPassword') where user='root';
mysql> flush privileges;
mysql> quit


$ ./bin/mysql -uroot -p
mysql> SET PASSWORD  = password('yourPassword');
mysql> flush privileges;
mysql> quit
```

<br/>

6. Open `.bashrc` and add MySQL installation path and adjust the modification.

```bash
$ vi ~/.bashrc

export PATH=/usr/local/mysql:$PATH
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/mysql/lib/

$ source ~/.bashrc
```

<br/>

7. Create `my.cnf` file. Change `/path/to/datadir/` to your local mysql data directory. In my case, it is `/home/vldb/datadir`. This file should be located at `usr/local/mysql/my.cnf`.

```bash
$ vi my.cnf

#
# The MySQL database server configuration file
#
[client]
user    = root
port    = 3306
socket  = /tmp/mysql.sock

[mysql]
prompt  = \u:\d>\_

[mysqld_safe]
socket  = /tmp/mysql.sock

[mysqld]
# Basic settings
default-storage-engine = innodb
pid-file        = /path/to/datadir/mysql.pid
socket          = /tmp/mysql.sock
port            = 3306
datadir         = /path/to/datadir/
log-error       = /path/to/datadir/mysql_error.log

#
# Innodb settings
#
# Page size
innodb_page_size=16KB

# Buffer pool settings
innodb_buffer_pool_size=1G
innodb_buffer_pool_instances=8

# Transaction log settings
innodb_log_file_size=100M
innodb_log_files_in_group=2
innodb_log_buffer_size=32M

# Log group path (iblog0, iblog1)
# If you separate the log device, uncomment and correct the path
#innodb_log_group_home_dir=/path/to/logdir/

# Flush settings (SSD-optimized)
# 0: every 1 seconds, 1: fsync on commits, 2: writes on commits
innodb_flush_log_at_trx_commit=0
innodb_flush_neighbors=0
innodb_flush_method=O_DIRECT
```

<br/>

8. Shut down the MySQL server and restart it with `my.cnf` file.

```bash
$ ./bin/mysqladmin -uroot -pyourPassword shutdown
$ ./bin/mysqld_safe --defaults-file=/usr/local/mysql/my.cnf
```

<br/><br/>

**Reference**

---
[SKKU3033-F2021 week-1 MySQL Installation](https://github.com/meeeejin/SWE3033-F2021/tree/main/week-1)
