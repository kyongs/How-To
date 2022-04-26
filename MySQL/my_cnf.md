my.cnf example1

```shell
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
pid-file        = /home/vldb/test-data/mysql.pid
socket          = /tmp/mysql.sock
port            = 3306
datadir         = /home/vldb/test-data/
log-error       = /home/vldb/test-log/mysql_error.log
log_error_verbosity = 3
disable_log_bin

#
# Innodb settings
#
# Page size
innodb_page_size=4KB

# Buffer pool settings
innodb_buffer_pool_size=1G
innodb_buffer_pool_instances=8

# Transaction log settings
innodb_log_file_size=100M
innodb_log_files_in_group=2
innodb_log_buffer_size=32M

# Log group path (iblog0, iblog1)
# If you separate the log device, uncomment and correct the path
innodb_log_group_home_dir=/home/vldb/test-log

# Flush settings (SSD-optimized)
# 0: every 1 seconds, 1: fsync on commits, 2: writes on commits
innodb_flush_log_at_trx_commit=0
innodb_flush_neighbors=0
innodb_flush_method=O_DIRECT
```
