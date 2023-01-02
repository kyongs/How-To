## tpcc-mysql Installation

### Prerequisite

---

```shell
$ apt-get install libmysqlclient-dev
```

### Installation

---

1. Clone `tpcc-mysql` github code.

```shell
$ git clone https://github.com/Percona-Lab/tpcc-mysql.git
```

2.

```shell
$ cd tpcc-mysql/src
$ make
```

3. Create `tpcc` database and tables. MySQL server must be running.

```bash
cd /usr/local/mysql

./bin/mysql -u root -p -e "CREATE DATABASE tpcc;"
./bin/mysql -u root -p tpcc < /home/vldb/tpcc-mysql/create_table.sql
./bin/mysql -u root -p tpcc < /home/vldb/tpcc-mysql/add_fkey_idx.sql
```

4. Load Data

```bash
cd ~/tpcc-mysql

./tpcc_load -h 127.0.0.1 -d tpcc -u root -p1234 -w 100 (-10G)
```

5. Run tpcc

```
./tpcc_start -h 127.0.0.1 -S /tmp/mysql.sock -d tpcc -u root -p1234 -w 100 -c 8 -r 300 -l 10800 | tee tpcc-result.txt
```
