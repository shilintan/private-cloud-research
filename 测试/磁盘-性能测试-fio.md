# 测试目标配置

8c32g



系统盘

50GiB

# FIO

参考文档: https://fio.readthedocs.io/en/latest/fio_doc.html

```
apt install -y fio
```

在纯净无干扰环境测试

## 16k-IODEPTH1-500M

### 操作

```
filename="/dev/vdb"
```



```
echo "顺序"
sleep 30
echo "顺序读"
fio --filename=${filename} --direct=1 -thread --rw=read --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30

echo "顺序写"
sleep 30
fio --filename=${filename} --direct=1 -thread --rw=write --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30

echo "顺序读写"
sleep 30
fio --filename=${filename} --direct=1 -thread --rw=rw --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30


echo "随机"
echo "随机读"
sleep 30
fio --filename=${filename} --direct=1 -thread --rw=randread --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30

echo "随机写"
sleep 30
fio --filename=${filename} --direct=1 -thread --rw=randwrite --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30

echo "随机读写"
sleep 30
fio --filename=${filename} --direct=1 -thread --rw=randrw --bs=16k -size=500M --ioengine=libaio --iodepth=1 --numjobs=1 --time_based --group_reporting --name=test-job --runtime=100 --eta-newline=30
```

### 结论

vdb

```
echo "顺序"
echo "顺序读"
最小0.02ms, 最大7ms, 平均0.03ms

echo "顺序写"
最小0.6ms, 最大14ms, 平均0.9ms

echo "顺序读写"
最小0.3ms, 最大8ms, 平均0.4ms
最小0.6ms, 最大10ms, 平均0.7ms


echo "随机"
echo "随机读"
最小0.3ms, 最大5ms, 平均0.5ms

echo "随机写"
最小0.6ms, 最大24ms, 平均1ms

echo "随机读写"
最小0.3ms, 最大5ms, 平均0.6ms
最小0.6ms, 最大30ms, 平均1ms
```



结论

```

```



### 过程

#### /dev/vdb

```
顺序
顺序读
test-job: (g=0): rw=read, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [R(1)][32.0%][r=521MiB/s][r=33.4k IOPS][eta 01m:08s]
Jobs: 1 (f=1): [R(1)][63.0%][r=544MiB/s][r=34.8k IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [R(1)][94.0%][r=495MiB/s][r=31.7k IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [R(1)][100.0%][r=541MiB/s][r=34.6k IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=60834: Thu Dec 21 02:41:03 2023
  read: IOPS=33.6k, BW=525MiB/s (550MB/s)(51.2GiB/100001msec)
    slat (usec): min=6, max=897, avg= 7.21, stdev= 1.35
    clat (nsec): min=470, max=6531.0k, avg=22179.45, stdev=19731.14
     lat (usec): min=18, max=6542, avg=29.39, stdev=19.91
    clat percentiles (usec):
     |  1.00th=[   15],  5.00th=[   15], 10.00th=[   15], 20.00th=[   15],
     | 30.00th=[   23], 40.00th=[   23], 50.00th=[   23], 60.00th=[   23],
     | 70.00th=[   24], 80.00th=[   24], 90.00th=[   25], 95.00th=[   30],
     | 99.00th=[   33], 99.50th=[   35], 99.90th=[  420], 99.95th=[  445],
     | 99.99th=[  578]
   bw (  KiB/s): min=371648, max=596928, per=100.00%, avg=537249.29, stdev=37659.62, samples=199
   iops        : min=23228, max=37308, avg=33578.08, stdev=2353.73, samples=199
  lat (nsec)   : 500=0.01%, 750=0.03%, 1000=0.01%
  lat (usec)   : 2=0.01%, 4=0.01%, 10=0.03%, 20=27.03%, 50=72.68%
  lat (usec)   : 100=0.02%, 250=0.01%, 500=0.19%, 750=0.01%, 1000=0.01%
  lat (msec)   : 2=0.01%, 4=0.01%, 10=0.01%
  cpu          : usr=5.84%, sys=19.33%, ctx=6714344, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=3357248,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=525MiB/s (550MB/s), 525MiB/s-525MiB/s (550MB/s-550MB/s), io=51.2GiB (55.0GB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=3353627/0, merge=0/0, ticks=82316/0, in_queue=82316, util=99.93%
顺序写
test-job: (g=0): rw=write, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [W(1)][32.0%][w=11.0MiB/s][w=707 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [W(1)][63.0%][w=21.2MiB/s][w=1360 IOPS][eta 00m:37s]
Jobs: 1 (f=1): [W(1)][94.0%][w=22.0MiB/s][w=1410 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [W(1)][100.0%][w=22.0MiB/s][w=1407 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=60935: Thu Dec 21 02:43:13 2023
  write: IOPS=1095, BW=17.1MiB/s (17.9MB/s)(1712MiB/100001msec); 0 zone resets
    slat (usec): min=5, max=151, avg= 7.38, stdev= 1.76
    clat (usec): min=567, max=14330, avg=904.74, stdev=326.78
     lat (usec): min=574, max=14337, avg=912.12, stdev=326.88
    clat percentiles (usec):
     |  1.00th=[  603],  5.00th=[  627], 10.00th=[  644], 20.00th=[  668],
     | 30.00th=[  693], 40.00th=[  709], 50.00th=[  734], 60.00th=[  766],
     | 70.00th=[  955], 80.00th=[ 1336], 90.00th=[ 1385], 95.00th=[ 1434],
     | 99.00th=[ 1614], 99.50th=[ 1729], 99.90th=[ 2008], 99.95th=[ 2376],
     | 99.99th=[ 5014]
   bw (  KiB/s): min=11104, max=23520, per=99.92%, avg=17512.04, stdev=5287.35, samples=199
   iops        : min=  694, max= 1470, avg=1094.50, stdev=330.46, samples=199
  lat (usec)   : 750=55.16%, 1000=15.11%
  lat (msec)   : 2=29.63%, 4=0.08%, 10=0.01%, 20=0.01%
  cpu          : usr=0.31%, sys=1.25%, ctx=109654, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,109542,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=17.1MiB/s (17.9MB/s), 17.1MiB/s-17.1MiB/s (17.9MB/s-17.9MB/s), io=1712MiB (1795MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=51/109401, merge=0/0, ticks=12/99076, in_queue=99089, util=99.96%
顺序读写
test-job: (g=0): rw=rw, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [M(1)][32.0%][r=13.3MiB/s,w=13.1MiB/s][r=852,w=839 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [M(1)][63.0%][r=13.3MiB/s,w=13.5MiB/s][r=848,w=862 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [M(1)][94.0%][r=12.0MiB/s,w=12.5MiB/s][r=766,w=797 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [M(1)][100.0%][r=12.9MiB/s,w=13.2MiB/s][r=828,w=848 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=61078: Thu Dec 21 02:45:24 2023
  read: IOPS=838, BW=13.1MiB/s (13.7MB/s)(1310MiB/100001msec)
    slat (usec): min=3, max=321, avg= 9.18, stdev= 1.88
    clat (usec): min=118, max=7533, avg=440.63, stdev=67.41
     lat (usec): min=270, max=7543, avg=449.81, stdev=67.46
    clat percentiles (usec):
     |  1.00th=[  322],  5.00th=[  359], 10.00th=[  379], 20.00th=[  400],
     | 30.00th=[  412], 40.00th=[  424], 50.00th=[  437], 60.00th=[  449],
     | 70.00th=[  465], 80.00th=[  482], 90.00th=[  506], 95.00th=[  529],
     | 99.00th=[  578], 99.50th=[  619], 99.90th=[  742], 99.95th=[  873],
     | 99.99th=[ 1926]
   bw (  KiB/s): min=11264, max=15200, per=100.00%, avg=13417.49, stdev=673.94, samples=199
   iops        : min=  704, max=  950, avg=838.59, stdev=42.12, samples=199
  write: IOPS=835, BW=13.0MiB/s (13.7MB/s)(1305MiB/100001msec); 0 zone resets
    slat (usec): min=5, max=323, avg= 7.69, stdev= 1.86
    clat (usec): min=567, max=10644, avg=736.94, stdev=94.84
     lat (usec): min=579, max=10651, avg=744.62, stdev=94.91
    clat percentiles (usec):
     |  1.00th=[  611],  5.00th=[  635], 10.00th=[  652], 20.00th=[  676],
     | 30.00th=[  693], 40.00th=[  717], 50.00th=[  734], 60.00th=[  742],
     | 70.00th=[  758], 80.00th=[  783], 90.00th=[  807], 95.00th=[  848],
     | 99.00th=[ 1057], 99.50th=[ 1139], 99.90th=[ 1352], 99.95th=[ 1483],
     | 99.99th=[ 3130]
   bw (  KiB/s): min=11744, max=15008, per=100.00%, avg=13364.74, stdev=539.13, samples=199
   iops        : min=  734, max=  938, avg=835.30, stdev=33.70, samples=199
  lat (usec)   : 250=0.01%, 500=44.29%, 750=37.44%, 1000=17.50%
  lat (msec)   : 2=0.76%, 4=0.01%, 10=0.01%, 20=0.01%
  cpu          : usr=0.51%, sys=1.44%, ctx=251283, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=83824,83512,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=13.1MiB/s (13.7MB/s), 13.1MiB/s-13.1MiB/s (13.7MB/s-13.7MB/s), io=1310MiB (1373MB), run=100001-100001msec
  WRITE: bw=13.0MiB/s (13.7MB/s), 13.0MiB/s-13.0MiB/s (13.7MB/s-13.7MB/s), io=1305MiB (1368MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=83791/83424, merge=0/0, ticks=37176/61554, in_queue=98730, util=99.96%
随机
随机读
test-job: (g=0): rw=randread, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [r(1)][32.0%][r=29.2MiB/s][r=1867 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [r(1)][63.0%][r=29.3MiB/s][r=1875 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [r(1)][94.0%][r=28.9MiB/s][r=1850 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [r(1)][100.0%][r=28.9MiB/s][r=1846 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=61179: Thu Dec 21 02:47:34 2023
  read: IOPS=1843, BW=28.8MiB/s (30.2MB/s)(2881MiB/100001msec)
    slat (usec): min=3, max=180, avg= 9.30, stdev= 1.79
    clat (usec): min=302, max=5469, avg=532.43, stdev=82.61
     lat (usec): min=325, max=5478, avg=541.73, stdev=82.64
    clat percentiles (usec):
     |  1.00th=[  400],  5.00th=[  433], 10.00th=[  453], 20.00th=[  478],
     | 30.00th=[  494], 40.00th=[  510], 50.00th=[  529], 60.00th=[  545],
     | 70.00th=[  562], 80.00th=[  586], 90.00th=[  619], 95.00th=[  644],
     | 99.00th=[  709], 99.50th=[  742], 99.90th=[  865], 99.95th=[ 1270],
     | 99.99th=[ 3163]
   bw (  KiB/s): min=26240, max=32416, per=100.00%, avg=29508.02, stdev=1004.06, samples=199
   iops        : min= 1640, max= 2026, avg=1844.25, stdev=62.75, samples=199
  lat (usec)   : 500=33.32%, 750=66.27%, 1000=0.34%
  lat (msec)   : 2=0.03%, 4=0.03%, 10=0.01%
  cpu          : usr=0.56%, sys=1.24%, ctx=368794, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=184359,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=28.8MiB/s (30.2MB/s), 28.8MiB/s-28.8MiB/s (30.2MB/s-30.2MB/s), io=2881MiB (3021MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=184170/0, merge=0/0, ticks=98595/0, in_queue=98595, util=99.95%
随机写
test-job: (g=0): rw=randwrite, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [w(1)][32.0%][w=17.0MiB/s][w=1088 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [w(1)][63.0%][w=17.2MiB/s][w=1104 IOPS][eta 00m:37s]
Jobs: 1 (f=1): [w(1)][94.0%][w=16.5MiB/s][w=1058 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [w(1)][100.0%][w=15.6MiB/s][w=1000 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=61277: Thu Dec 21 02:49:44 2023
  write: IOPS=1018, BW=15.9MiB/s (16.7MB/s)(1592MiB/100001msec); 0 zone resets
    slat (usec): min=5, max=122, avg= 7.30, stdev= 1.66
    clat (usec): min=619, max=23666, avg=973.67, stdev=139.10
     lat (usec): min=627, max=23673, avg=980.97, stdev=139.14
    clat percentiles (usec):
     |  1.00th=[  775],  5.00th=[  832], 10.00th=[  865], 20.00th=[  898],
     | 30.00th=[  922], 40.00th=[  947], 50.00th=[  963], 60.00th=[  988],
     | 70.00th=[ 1012], 80.00th=[ 1037], 90.00th=[ 1090], 95.00th=[ 1123],
     | 99.00th=[ 1303], 99.50th=[ 1401], 99.90th=[ 1680], 99.95th=[ 2180],
     | 99.99th=[ 3523]
   bw (  KiB/s): min=13408, max=17824, per=100.00%, avg=16305.69, stdev=624.07, samples=199
   iops        : min=  838, max= 1114, avg=1019.11, stdev=39.00, samples=199
  lat (usec)   : 750=0.50%, 1000=65.26%
  lat (msec)   : 2=34.18%, 4=0.05%, 10=0.01%, 20=0.01%, 50=0.01%
  cpu          : usr=0.29%, sys=1.10%, ctx=102011, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,101866,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=15.9MiB/s (16.7MB/s), 15.9MiB/s-15.9MiB/s (16.7MB/s-16.7MB/s), io=1592MiB (1669MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=51/101765, merge=0/0, ticks=13/99186, in_queue=99200, util=99.95%
随机读写
test-job: (g=0): rw=randrw, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [m(1)][32.0%][r=9536KiB/s,w=9.88MiB/s][r=596,w=632 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [m(1)][63.0%][r=9529KiB/s,w=9753KiB/s][r=595,w=609 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [m(1)][94.0%][r=9.88MiB/s,w=9.88MiB/s][r=632,w=632 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [m(1)][100.0%][r=9.77MiB/s,w=9936KiB/s][r=625,w=621 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=61379: Thu Dec 21 02:51:54 2023
  read: IOPS=627, BW=9.80MiB/s (10.3MB/s)(980MiB/100001msec)
    slat (usec): min=2, max=290, avg= 9.64, stdev= 2.48
    clat (usec): min=340, max=4548, avg=567.90, stdev=80.02
     lat (usec): min=349, max=4560, avg=577.55, stdev=80.12
    clat percentiles (usec):
     |  1.00th=[  437],  5.00th=[  469], 10.00th=[  490], 20.00th=[  515],
     | 30.00th=[  529], 40.00th=[  545], 50.00th=[  562], 60.00th=[  578],
     | 70.00th=[  594], 80.00th=[  619], 90.00th=[  644], 95.00th=[  676],
     | 99.00th=[  734], 99.50th=[  766], 99.90th=[  922], 99.95th=[ 1434],
     | 99.99th=[ 2966]
   bw (  KiB/s): min= 8416, max=11584, per=100.00%, avg=10042.69, stdev=622.02, samples=199
   iops        : min=  526, max=  724, avg=627.67, stdev=38.88, samples=199
  write: IOPS=624, BW=9997KiB/s (10.2MB/s)(976MiB/100001msec); 0 zone resets
    slat (nsec): min=5790, max=97627, avg=8024.80, stdev=1676.70
    clat (usec): min=642, max=30800, avg=1011.02, stdev=169.42
     lat (usec): min=649, max=30807, avg=1019.04, stdev=169.45
    clat percentiles (usec):
     |  1.00th=[  799],  5.00th=[  865], 10.00th=[  889], 20.00th=[  930],
     | 30.00th=[  955], 40.00th=[  979], 50.00th=[ 1004], 60.00th=[ 1029],
     | 70.00th=[ 1057], 80.00th=[ 1074], 90.00th=[ 1123], 95.00th=[ 1172],
     | 99.00th=[ 1336], 99.50th=[ 1418], 99.90th=[ 1745], 99.95th=[ 2245],
     | 99.99th=[ 3884]
   bw (  KiB/s): min= 8224, max=11200, per=100.00%, avg=10001.05, stdev=496.03, samples=199
   iops        : min=  514, max=  700, avg=625.07, stdev=31.00, samples=199
  lat (usec)   : 500=6.88%, 750=42.94%, 1000=24.74%
  lat (msec)   : 2=25.38%, 4=0.05%, 10=0.01%, 50=0.01%
  cpu          : usr=0.44%, sys=1.11%, ctx=187985, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=62734,62483,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=9.80MiB/s (10.3MB/s), 9.80MiB/s-9.80MiB/s (10.3MB/s-10.3MB/s), io=980MiB (1028MB), run=100001-100001msec
  WRITE: bw=9997KiB/s (10.2MB/s), 9997KiB/s-9997KiB/s (10.2MB/s-10.2MB/s), io=976MiB (1024MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=62721/62417, merge=0/0, ticks=35803/63157, in_queue=98959, util=99.96%
```



```
最小0.2ms, 最大9ms, 平均0.5ms
最小0.3ms, 最大8ms, 平均0.4ms
```



