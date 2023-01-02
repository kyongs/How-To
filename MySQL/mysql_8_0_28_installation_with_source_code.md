## Build and Install the MySQL source code (8.0)

This document is based on Ubuntu 18.04, MySQL 8.0.28
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

# for mysql8.0.31, you have to install libboost-dev
$ sudo apt-get install libboost-dev

```

### Build and Install

---

1. Group, user configuration

```bash
$ sudo groupadd mysql
$ sudo useradd -r -g mysql -s /bin/false mysql
```

<br/>

2. Download the source code from [MySQL download URL.](https://dev.mysql.com/downloads/mysql/) (**Select OS**: Source Code, **Select OS Version**: All Operating Systems)

```bash
$ wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.28.tar.gz
```

<br/>

3. Unzip the `tar.gz` file and make the build file inside the source code. Follow the error message if the boost file does not exist in your MySQL directory. <br/> You would have to add `-DWITH` option and `-DDOWNLOAD` option with the `cmake` operation. I executed the `make` command by allocating 16 CPUs by `-j16` option. This is optional. (It will take much time if you get rid of this option, though.)

```bash
$ tar zxvf mysql-8.0.28.tar.gz
$ cd mysql-8.0.28
$ mkdir bld
$ cd bld
$ sudo cmake ..
# if no boost file: 
# $ sudo cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST={path_to_mysql_code}/mysql-8.0.28/bld
$ sudo make -j16 install
```

<br/>

4. MySQL makes the base directory in `/usr/local/mysql` by default. In this document, I will make the data directory(`test-data`) at the `home` directory. Initialize MySQL with the following command.

```shell
$ cd /usr/local/mysql
$ ./bin/mysqld --initialize --user=mysql --datadir=/home/{usr_name}/test-data --basedir=/usr/local/mysql
```

<br/>

5. Set password. Run the MySQL Server with `--skip-grant-tables` command. You can check with `ps -ef | grep mysql` command if MySQL server is operating.

```bash
$ ./bin/mysqld_safe --skip-grant-tables --datadir=/home/{usr_name}/test-data
```

on the other terminal window or tab (location: /usr/local/mysql)

```bash
# check mysql if it is running correctly
$ ps -ef | grep mysql

$ ./bin/mysql -uroot

mysql> use mysql;
mysql> UPDATE mysql.user SET authentication_string=null WHERE User='root';
mysql> flush privileges;
mysql> quit


$ ./bin/mysql -uroot -p
# just press <enter> because we set the root password to 'null' at the forward process.
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '1234';
mysql> SET PASSWORD FOR 'root'@'localhost' = '1234';
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
$ ./bin/mysqladmin -uroot -p1234 shutdown
$ ./bin/mysqld_safe --defaults-file=/usr/local/mysql/my.cnf
```

<br/><br/>

**Reference**

---

[Building MySQL from source](https://downloads.mysql.com/docs/mysql-sourcebuild-excerpt-8.0-en.pdf) <br/>
[SKKU3033-F2021 week-1 MySQL Installation](https://github.com/meeeejin/SWE3033-F2021/tree/main/week-1)
