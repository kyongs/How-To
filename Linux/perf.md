## How To Run Perf in Linux and Flame Graph

#### 1. Download the Perf

```Shell
$ sudo apt-get install linux-tools-common linux-tools-generic linux-tools-`uname -r`
```

#### 2. Download Flame Graph
```
$ git clone https://github.com/brendangregg/FlameGraph
$ cd FlameGraph
```


#### 3. Run perf
```Shell
$ sudo perf record -F 99 -p {프로세스ID} -g -- sleep {몇초동안}
```

#### 4. Change the perf result to flame graph
```Shell
$ sudo perf script > out.perf
$ ./stackcollapse-perf.pl out.perf > out.folded
$ ./flamegraph.pl out.folded > test.svg


```
