### How to execute FIO benchmark




- Install FIO
  ```bash
   $ sudo apt-get update && sudo apt-get upgrade
   $ apt-get install fio
  ```

- The purpose of FIO benchmark is to evaluate bandwidth of storage device.

- Test Script
  - Format
    ```bash
    [global]
    global-parameter-1=value
    global-parameter-2=value
    global-parameter-3=value
    ...

    [1st test]
    stonewall
    1st-test-parameter-1=value
    1st-test-parameter-2=value
    1st-test-parameter-3=value
    ...

    [2nd test]
    stonewall
    2nd-test-paramenter-1=value
    ...

    ```
  - Example
    ```bash
    [global]
    ioengine=libaio
    filename=/dev/nvme1n1
    group_reporting=1
    direct=1
    verify=0
    norandommap=1
    time_based=1
    runtime=1800s
    ramp_time=10
    randrepeat=0
    refill_buffers
    log_avg_msec=1000
    log_max_value=1
    unified_rw_reporting=1
    percentile_list=50:99:99.9:99.99:99.999

    [4k_fill_space]
    stonewall
    bs=4k
    rw=randwrite
    iodepth=32
    numjobs=8
    size=500G
    ```


- You can use FIO command regardless of whether the device is mounted or not. 
- Useful FIO options

  | Trace Statistics      | Description |
  |-----------------------|-------------|
  | name=[string]         | test name   |
  | rw=[IO type]          | write / read / randwrite / randread / readwrite / randrw |
  | numjobs=[int]         | job #       |
  | ioengine=[IO method]  | IO engine             |
  | ramp_time=[int]       | time between tests    | 
  | iodepth=[int]         | queue depth           |
  | time_based            |   |
  | direct=[bool]         | true: direct IO / false: Buffered IO |
  
    



- FIO benchmark has numerous options. Therefore, refer to the manual page for detailed execution. 
  [https://linux.die.net/man/1/fio](https://linux.die.net/man/1/fio)
  
- ㅇ
  ```bash
  4k_randwrite: (g=0): rw=randwrite, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=32
  ...
  4k_randread: (g=1): rw=randread, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=32
  ...
  fio-3.1
  Starting 8 processes

  4k_randwrite: (groupid=0, jobs=4): err= 0: pid=10347: Wed May 31 18:15:30 2023
    mixed: IOPS=165k, BW=643MiB/s (674MB/s)(1130GiB/1800001msec)
      slat (nsec): min=1943, max=2600.4k, avg=6969.16, stdev=4526.14
      clat (usec): min=97, max=35900, avg=767.45, stdev=589.12
       lat (usec): min=100, max=35907, avg=774.63, stdev=590.51
      clat percentiles (usec):
       | 50.000th=[  611], 99.000th=[ 2933], 99.900th=[ 3556], 99.990th=[ 4424],
       | 99.999th=[ 6915]
     bw (  KiB/s): min=57045, max=339820, per=20.22%, avg=133081.34, stdev=56901.48, samples=14396
     iops        : min=14261, max=84955, avg=33269.97, stdev=14225.37, samples=14396
    lat (usec)   : 100=0.01%, 250=12.01%, 500=28.49%, 750=22.35%, 1000=14.23%
    lat (msec)   : 2=17.71%, 4=5.18%, 10=0.02%, 20=0.01%, 50=0.01%
    cpu          : usr=33.18%, sys=27.76%, ctx=37456450, majf=0, minf=12
    IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=101.1%, >=64=0.0%
       submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
       complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.1%, 64=0.0%, >=64=0.0%
       issued rwt: total=296139504,0,0, short=0,0,0, dropped=0,0,0
       latency   : target=0, window=0, percentile=100.00%, depth=32
  4k_randread: (groupid=1, jobs=4): err= 0: pid=10483: Wed May 31 18:15:30 2023
    mixed: IOPS=452k, BW=1765MiB/s (1851MB/s)(3102GiB/1800001msec)
      slat (nsec): min=1863, max=562891, avg=3058.38, stdev=1396.36
      clat (usec): min=6, max=5117, avg=279.62, stdev=90.93
       lat (usec): min=10, max=5120, avg=282.77, stdev=90.95
      clat percentiles (usec):
       | 50.000th=[  289], 99.000th=[  519], 99.900th=[  791], 99.990th=[ 1074],
       | 99.999th=[ 1352]
     bw (  KiB/s): min=211895, max=677496, per=25.01%, avg=452058.82, stdev=72274.49, samples=14400
     iops        : min=52973, max=169374, avg=113014.67, stdev=18068.62, samples=14400
    lat (usec)   : 10=0.01%, 20=0.20%, 50=2.34%, 100=3.55%, 250=16.05%
    lat (usec)   : 500=76.67%, 750=1.06%, 1000=0.12%
    lat (msec)   : 2=0.02%, 4=0.01%, 10=0.01%
    cpu          : usr=29.82%, sys=37.53%, ctx=256548190, majf=0, minf=9
    IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=100.5%, >=64=0.0%
       submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
       complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.1%, 64=0.0%, >=64=0.0%
       issued rwt: total=813264422,0,0, short=0,0,0, dropped=0,0,0
       latency   : target=0, window=0, percentile=100.00%, depth=32

  Run status group 0 (all jobs):
    MIXED: bw=643MiB/s (674MB/s), 643MiB/s-643MiB/s (674MB/s-674MB/s), io=1130GiB (1213GB), run=1800001-1800001msec

  Run status group 1 (all jobs):
    MIXED: bw=1765MiB/s (1851MB/s), 1765MiB/s-1765MiB/s (1851MB/s-1851MB/s), io=3102GiB (3331GB), run=1800001-1800001msec
  ```


