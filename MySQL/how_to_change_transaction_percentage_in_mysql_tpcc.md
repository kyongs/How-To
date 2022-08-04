## How to Change Transaction Percentage in tpcc-mysql


#### 1. Go to main.c in tpcc-mysql/src and change `seq_init` parameter.
```c

  if(valuable_flg==0){
    // seq_init(10,10,1,1,1); /* normal ratio */
    seq_init(10,10,1,0,1); //<<<-remove delivery
  }else{
    seq_init( atoi(argv[9 + arg_offset]), atoi(argv[10 + arg_offset]), atoi(argv[11 + arg_offset]),
	      atoi(argv[12 + arg_offset]), atoi(argv[13 + arg_offset]) );
  }
```

#### 2. make
```bash
$ cd tpcc-mysql/src
$ make clean
$ make
```

#### 3. Run MySQL
```bash
$ cd /usr/local/mysql
$ ./bin/mysqld_safe --defaults-file=/usr/local/mysql/my.cnf
2022-08-04T05:24:09.807833Z mysqld_safe Logging to '/home/vldb/log-data/mysql_error.log'.
2022-08-04T05:24:09.856019Z mysqld_safe Starting mysqld daemon with databases from /home/vldb/data-disk/test-data
```

#### 4. Run TPC-C
```bash
$ cd ~/tpcc-mysql
$ ./tpcc_start -h 127.0.0.1 -S /tmp/mysql.sock -d tpcc -u root -p1234 -w 100 -c 8 -r 300 -l 1200 | tee tpcc-result-ol.txt


***************************************
*** ###easy### TPC-C Load Generator ***
***************************************
option h with value '127.0.0.1'
option S (socket) with value '/tmp/mysql.sock'
option d with value 'tpcc'
option u with value 'root'
option p with value '1234'
option w with value '100'
option c with value '8'
option r with value '300'
option l with value '1200'
<Parameters>
     [server]: 127.0.0.1
     [port]: 3306
     [DBname]: tpcc
       [user]: root
       [pass]: 1234
  [warehouse]: 100
 [connection]: 8
     [rampup]: 300 (sec.)
    [measure]: 1200 (sec.)

RAMP-UP TIME.(300 sec.)

MEASURING START.

  10, trx: 4607, 95%: 25.619, 99%: 33.360, max_rt: 68.534, 4605|56.758, 460|33.914, 0|0.000, 462|210.596
  20, trx: 4644, 95%: 26.271, 99%: 33.792, max_rt: 49.841, 4643|19.886, 464|12.757, 0|0.000, 464|171.668
  30, trx: 4565, 95%: 27.110, 99%: 34.840, max_rt: 49.614, 4563|27.987, 457|30.750, 0|0.000, 457|188.587
  40, trx: 4701, 95%: 26.916, 99%: 35.154, max_rt: 54.056, 4696|36.901, 470|18.244, 0|0.000, 471|191.524
  50, trx: 4719, 95%: 27.232, 99%: 34.756, max_rt: 48.987, 4722|22.321, 472|17.084, 0|0.000, 470|192.898
  60, trx: 4563, 95%: 27.552, 99%: 33.833, max_rt: 60.348, 4562|25.595, 456|22.703, 0|0.000, 457|176.911
  70, trx: 4717, 95%: 26.013, 99%: 33.022, max_rt: 47.486, 4717|12.987, 472|20.225, 0|0.000, 472|171.526
  80, trx: 4694, 95%: 26.469, 99%: 33.893, max_rt: 45.049, 4691|29.316, 469|13.630, 0|0.000, 469|234.206
  90, trx: 4637, 95%: 26.812, 99%: 33.661, max_rt: 47.958, 4638|21.327, 464|15.580, 0|0.000, 463|197.113
 100, trx: 4588, 95%: 26.636, 99%: 34.704, max_rt: 47.383, 4591|38.835, 459|18.417, 0|0.000, 461|163.849
 110, trx: 4755, 95%: 26.311, 99%: 34.788, max_rt: 51.928, 4758|22.056, 475|18.110, 0|0.000, 475|185.552
 120, trx: 4683, 95%: 27.281, 99%: 35.429, max_rt: 44.117, 4676|22.020, 469|19.023, 0|0.000, 468|156.014
 ...
```
