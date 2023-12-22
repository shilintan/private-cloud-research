# 测试目标配置

8c32g



系统盘

ESSD云盘 50GiB

随实例释放

PL0（单盘IOPS性能上限1万）



数据盘

ESSD云盘 1024GiB

随实例释放

PL2（单盘IOPS性能上限10万）

# FIO

参考文档: https://fio.readthedocs.io/en/latest/fio_doc.html

```
apt install -y fio
```

在纯净无干扰环境测试

## 16k-IODEPTH1-500M

### 操作

```
filename="/dev/vda"
```

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

vda

```
echo "顺序"
echo "顺序读"
最小0.2ms, 最大50ms, 平均0.4ms

echo "顺序写"
最小0.3ms, 最大16ms, 平均0.4ms

echo "顺序读写"
最小0.2ms, 最大16ms, 平均0.4ms
最小0.3ms, 最大8ms, 平均0.5ms


echo "随机"
echo "随机读"
最小0.2ms, 最大6ms, 平均0.4ms

echo "随机写"
最小0.3ms, 最大7ms, 平均0.4ms

echo "随机读写"
最小0.2ms, 最大9ms, 平均0.5ms
最小0.3ms, 最大8ms, 平均0.4ms
```



vdb

```
echo "顺序"
echo "顺序读"
最小0.07ms, 最大7ms, 平均0.1ms

echo "顺序写"
最小0.1ms, 最大86ms, 平均0.2ms

echo "顺序读写"
最小0.07ms, 最大8ms, 平均0.1ms
最小0.1ms, 最大7ms, 平均0.1ms


echo "随机"
echo "随机读"
最小0.08ms, 最大14ms, 平均0.3ms

echo "随机写"
最小0.1ms, 最大17ms, 平均0.2ms

echo "随机读写"
最小0.08ms, 最大10ms, 平均0.3ms
最小0.1ms, 最大5ms, 平均0.2ms
```



结论

```
云磁盘越大, 延迟越低
```



### 过程

#### /dev/vda

```
Jobs: 1 (f=1): [R(1)][100.0%][r=37.5MiB/s][r=2400 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2241: Thu Dec 21 13:11:20 2023
  read: IOPS=2425, BW=37.9MiB/s (39.7MB/s)(3790MiB/100001msec)
    slat (nsec): min=1470, max=107409, avg=1656.99, stdev=384.35
    clat (usec): min=197, max=50455, avg=410.35, stdev=927.40
     lat (usec): min=198, max=50457, avg=412.01, stdev=927.40
    clat percentiles (usec):
     |  1.00th=[  208],  5.00th=[  212], 10.00th=[  217], 20.00th=[  219],
     | 30.00th=[  221], 40.00th=[  225], 50.00th=[  227], 60.00th=[  231],
     | 70.00th=[  235], 80.00th=[  239], 90.00th=[  258], 95.00th=[  297],
     | 99.00th=[ 4817], 99.50th=[ 4883], 99.90th=[ 8979], 99.95th=[ 9241],
     | 99.99th=[ 9372]
   bw (  KiB/s): min=37632, max=69408, per=100.00%, avg=38816.96, stdev=3419.06, samples=199
   iops        : min= 2352, max= 4338, avg=2426.06, stdev=213.69, samples=199
  lat (usec)   : 250=87.40%, 500=8.53%, 750=0.18%, 1000=0.05%
  lat (msec)   : 2=0.03%, 4=0.08%, 10=3.73%, 50=0.01%, 100=0.01%
  cpu          : usr=0.28%, sys=0.81%, ctx=242665, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=242543,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=37.9MiB/s (39.7MB/s), 37.9MiB/s-37.9MiB/s (39.7MB/s-39.7MB/s), io=3790MiB (3974MB), run=100001-100001msec

Disk stats (read/write):
  vda: ios=242302/48, merge=0/17, ticks=99186/75, in_queue=99262, util=99.93%
```



```
Jobs: 1 (f=1): [W(1)][32.0%][w=37.5MiB/s][w=2398 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [W(1)][63.0%][w=37.5MiB/s][w=2400 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [W(1)][94.0%][w=37.5MiB/s][w=2401 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [W(1)][100.0%][w=37.5MiB/s][w=2398 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2285: Thu Dec 21 13:18:55 2023
  write: IOPS=2425, BW=37.9MiB/s (39.7MB/s)(3790MiB/100001msec); 0 zone resets
    slat (nsec): min=1469, max=20288, avg=1828.93, stdev=326.85
    clat (usec): min=292, max=15686, avg=410.06, stdev=380.50
     lat (usec): min=294, max=15688, avg=411.89, stdev=380.49
    clat percentiles (usec):
     |  1.00th=[  343],  5.00th=[  347], 10.00th=[  347], 20.00th=[  351],
     | 30.00th=[  351], 40.00th=[  351], 50.00th=[  355], 60.00th=[  355],
     | 70.00th=[  359], 80.00th=[  363], 90.00th=[  396], 95.00th=[  474],
     | 99.00th=[ 2737], 99.50th=[ 3851], 99.90th=[ 5276], 99.95th=[ 5538],
     | 99.99th=[ 5997]
   bw (  KiB/s): min=37152, max=44000, per=100.00%, avg=38816.96, stdev=1426.69, samples=199
   iops        : min= 2322, max= 2750, avg=2426.06, stdev=89.17, samples=199
  lat (usec)   : 500=96.75%, 750=1.26%, 1000=0.09%
  lat (msec)   : 2=0.84%, 4=0.65%, 10=0.41%, 20=0.01%
  cpu          : usr=0.39%, sys=0.79%, ctx=242625, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,242537,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=37.9MiB/s (39.7MB/s), 37.9MiB/s-37.9MiB/s (39.7MB/s-39.7MB/s), io=3790MiB (3974MB), run=100001-100001msec

Disk stats (read/write):
  vda: ios=13/242335, merge=0/9, ticks=7/99146, in_queue=99153, util=99.93%
```



```
Jobs: 1 (f=1): [M(1)][32.0%][r=19.2MiB/s,w=18.3MiB/s][r=1228,w=1171 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [M(1)][63.0%][r=18.7MiB/s,w=18.8MiB/s][r=1195,w=1205 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [M(1)][94.0%][r=18.4MiB/s,w=19.3MiB/s][r=1177,w=1235 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [M(1)][100.0%][r=18.2MiB/s,w=19.2MiB/s][r=1168,w=1229 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2310: Thu Dec 21 13:22:02 2023
  read: IOPS=1213, BW=19.0MiB/s (19.9MB/s)(1896MiB/100001msec)
    slat (nsec): min=1474, max=17843, avg=1694.75, stdev=313.40
    clat (usec): min=201, max=15664, avg=351.54, stdev=684.60
     lat (usec): min=203, max=15666, avg=353.23, stdev=684.60
    clat percentiles (usec):
     |  1.00th=[  212],  5.00th=[  217], 10.00th=[  219], 20.00th=[  223],
     | 30.00th=[  225], 40.00th=[  229], 50.00th=[  231], 60.00th=[  235],
     | 70.00th=[  239], 80.00th=[  249], 90.00th=[  269], 95.00th=[  371],
     | 99.00th=[ 4817], 99.50th=[ 5735], 99.90th=[ 6652], 99.95th=[ 7177],
     | 99.99th=[ 7701]
   bw (  KiB/s): min=17888, max=26592, per=100.00%, avg=19421.11, stdev=1197.24, samples=199
   iops        : min= 1118, max= 1662, avg=1213.82, stdev=74.83, samples=199
  write: IOPS=1212, BW=18.9MiB/s (19.9MB/s)(1894MiB/100001msec); 0 zone resets
    slat (nsec): min=1496, max=14963, avg=1863.02, stdev=351.60
    clat (usec): min=308, max=8317, avg=468.74, stdev=658.42
     lat (usec): min=309, max=8319, avg=470.60, stdev=658.42
    clat percentiles (usec):
     |  1.00th=[  343],  5.00th=[  343], 10.00th=[  347], 20.00th=[  347],
     | 30.00th=[  351], 40.00th=[  351], 50.00th=[  355], 60.00th=[  355],
     | 70.00th=[  359], 80.00th=[  363], 90.00th=[  416], 95.00th=[  494],
     | 99.00th=[ 4293], 99.50th=[ 5800], 99.90th=[ 6718], 99.95th=[ 7111],
     | 99.99th=[ 7701]
   bw (  KiB/s): min=17600, max=26080, per=100.00%, avg=19397.31, stdev=1226.10, samples=199
   iops        : min= 1100, max= 1630, avg=1212.33, stdev=76.63, samples=199
  lat (usec)   : 250=40.74%, 500=55.02%, 750=1.14%, 1000=0.17%
  lat (msec)   : 2=0.10%, 4=1.76%, 10=1.06%, 20=0.01%
  cpu          : usr=0.27%, sys=0.89%, ctx=242655, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=121339,121211,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=19.0MiB/s (19.9MB/s), 19.0MiB/s-19.0MiB/s (19.9MB/s-19.9MB/s), io=1896MiB (1988MB), run=100001-100001msec
  WRITE: bw=18.9MiB/s (19.9MB/s), 18.9MiB/s-18.9MiB/s (19.9MB/s-19.9MB/s), io=1894MiB (1986MB), run=100001-100001msec

Disk stats (read/write):
  vda: ios=121229/121131, merge=0/4, ticks=42495/56731, in_queue=99226, util=99.93%
```



```
Jobs: 1 (f=1): [r(1)][32.0%][r=38.4MiB/s][r=2459 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [r(1)][63.0%][r=37.6MiB/s][r=2407 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [r(1)][94.0%][r=37.5MiB/s][r=2398 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [r(1)][100.0%][r=37.5MiB/s][r=2397 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2327: Thu Dec 21 13:24:48 2023
  read: IOPS=2425, BW=37.9MiB/s (39.7MB/s)(3790MiB/100001msec)
    slat (nsec): min=1371, max=20715, avg=1605.52, stdev=286.36
    clat (usec): min=207, max=5817, avg=410.20, stdev=126.41
     lat (usec): min=209, max=5819, avg=411.81, stdev=126.42
    clat percentiles (usec):
     |  1.00th=[  258],  5.00th=[  343], 10.00th=[  355], 20.00th=[  371],
     | 30.00th=[  379], 40.00th=[  388], 50.00th=[  396], 60.00th=[  408],
     | 70.00th=[  420], 80.00th=[  441], 90.00th=[  457], 95.00th=[  474],
     | 99.00th=[  676], 99.50th=[  832], 99.90th=[ 2212], 99.95th=[ 3556],
     | 99.99th=[ 4424]
   bw (  KiB/s): min=37792, max=41472, per=100.00%, avg=38820.50, stdev=604.44, samples=199
   iops        : min= 2362, max= 2592, avg=2426.28, stdev=37.78, samples=199
  lat (usec)   : 250=0.89%, 500=96.13%, 750=2.29%, 1000=0.36%
  lat (msec)   : 2=0.21%, 4=0.07%, 10=0.03%
  cpu          : usr=0.35%, sys=0.82%, ctx=242621, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=242557,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=37.9MiB/s (39.7MB/s), 37.9MiB/s-37.9MiB/s (39.7MB/s-39.7MB/s), io=3790MiB (3974MB), run=100001-100001msec

Disk stats (read/write):
  vda: ios=242314/14, merge=0/4, ticks=99095/6, in_queue=99101, util=99.92%
```



```
Jobs: 1 (f=1): [w(1)][32.0%][w=37.4MiB/s][w=2391 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [w(1)][63.0%][w=37.5MiB/s][w=2399 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [w(1)][94.0%][w=37.5MiB/s][w=2401 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [w(1)][100.0%][w=37.5MiB/s][w=2403 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2342: Thu Dec 21 13:28:11 2023
  write: IOPS=2425, BW=37.9MiB/s (39.7MB/s)(3790MiB/100002msec); 0 zone resets
    slat (nsec): min=1467, max=17756, avg=1823.75, stdev=338.56
    clat (usec): min=289, max=6544, avg=409.97, stdev=350.78
     lat (usec): min=290, max=6546, avg=411.79, stdev=350.78
    clat percentiles (usec):
     |  1.00th=[  343],  5.00th=[  347], 10.00th=[  347], 20.00th=[  351],
     | 30.00th=[  351], 40.00th=[  355], 50.00th=[  355], 60.00th=[  355],
     | 70.00th=[  359], 80.00th=[  363], 90.00th=[  412], 95.00th=[  486],
     | 99.00th=[ 2114], 99.50th=[ 3163], 99.90th=[ 5080], 99.95th=[ 5407],
     | 99.99th=[ 5800]
   bw (  KiB/s): min=38016, max=43840, per=100.00%, avg=38824.36, stdev=1372.30, samples=199
   iops        : min= 2376, max= 2740, avg=2426.52, stdev=85.77, samples=199
  lat (usec)   : 500=95.88%, 750=1.84%, 1000=0.28%
  lat (msec)   : 2=0.98%, 4=0.71%, 10=0.30%
  cpu          : usr=0.36%, sys=0.82%, ctx=242788, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,242585,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=37.9MiB/s (39.7MB/s), 37.9MiB/s-37.9MiB/s (39.7MB/s-39.7MB/s), io=3790MiB (3975MB), run=100002-100002msec

Disk stats (read/write):
  vda: ios=62/242345, merge=0/2, ticks=24/99093, in_queue=99117, util=99.95%
```



```
Jobs: 1 (f=1): [m(1)][32.0%][r=19.8MiB/s,w=18.5MiB/s][r=1266,w=1185 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [m(1)][63.0%][r=18.7MiB/s,w=18.9MiB/s][r=1197,w=1212 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [m(1)][94.0%][r=18.3MiB/s,w=19.2MiB/s][r=1170,w=1231 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [m(1)][100.0%][r=18.2MiB/s,w=19.2MiB/s][r=1166,w=1230 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2360: Thu Dec 21 13:31:01 2023
  read: IOPS=1213, BW=19.0MiB/s (19.9MB/s)(1896MiB/100001msec)
    slat (nsec): min=1499, max=20035, avg=1736.82, stdev=289.22
    clat (usec): min=208, max=8817, avg=450.45, stdev=138.55
     lat (usec): min=209, max=8819, avg=452.18, stdev=138.55
    clat percentiles (usec):
     |  1.00th=[  347],  5.00th=[  375], 10.00th=[  388], 20.00th=[  404],
     | 30.00th=[  416], 40.00th=[  424], 50.00th=[  437], 60.00th=[  449],
     | 70.00th=[  457], 80.00th=[  469], 90.00th=[  490], 95.00th=[  545],
     | 99.00th=[  824], 99.50th=[ 1012], 99.90th=[ 2278], 99.95th=[ 3687],
     | 99.99th=[ 4555]
   bw (  KiB/s): min=17952, max=21024, per=100.00%, avg=19420.62, stdev=589.15, samples=199
   iops        : min= 1122, max= 1314, avg=1213.79, stdev=36.82, samples=199
  write: IOPS=1212, BW=18.9MiB/s (19.9MB/s)(1894MiB/100001msec); 0 zone resets
    slat (nsec): min=1509, max=21025, avg=1857.99, stdev=293.49
    clat (usec): min=294, max=7820, avg=369.68, stdev=119.64
     lat (usec): min=296, max=7822, avg=371.53, stdev=119.64
    clat percentiles (usec):
     |  1.00th=[  343],  5.00th=[  343], 10.00th=[  347], 20.00th=[  347],
     | 30.00th=[  351], 40.00th=[  351], 50.00th=[  355], 60.00th=[  355],
     | 70.00th=[  359], 80.00th=[  363], 90.00th=[  383], 95.00th=[  453],
     | 99.00th=[  586], 99.50th=[  734], 99.90th=[ 1958], 99.95th=[ 3621],
     | 99.99th=[ 4293]
   bw (  KiB/s): min=17952, max=21088, per=100.00%, avg=19396.66, stdev=592.74, samples=199
   iops        : min= 1122, max= 1318, avg=1212.29, stdev=37.05, samples=199
  lat (usec)   : 250=0.10%, 500=94.72%, 750=4.24%, 1000=0.52%
  lat (msec)   : 2=0.30%, 4=0.09%, 10=0.02%
  cpu          : usr=0.28%, sys=0.93%, ctx=242607, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=121331,121203,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=19.0MiB/s (19.9MB/s), 19.0MiB/s-19.0MiB/s (19.9MB/s-19.9MB/s), io=1896MiB (1988MB), run=100001-100001msec
  WRITE: bw=18.9MiB/s (19.9MB/s), 18.9MiB/s-18.9MiB/s (19.9MB/s-19.9MB/s), io=1894MiB (1986MB), run=100001-100001msec

Disk stats (read/write):
  vda: ios=121220/121130, merge=0/7, ticks=54464/44691, in_queue=99156, util=99.94%
```



#### /dev/vdb

```
顺序
顺序读
test-job: (g=0): rw=read, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [R(1)][32.0%][r=158MiB/s][r=10.1k IOPS][eta 01m:08s]
Jobs: 1 (f=1): [R(1)][63.0%][r=166MiB/s][r=10.6k IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [R(1)][94.0%][r=160MiB/s][r=10.3k IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [R(1)][100.0%][r=153MiB/s][r=9768 IOPS][eta 00m:00s] 
test-job: (groupid=0, jobs=1): err= 0: pid=2392: Thu Dec 21 13:36:15 2023
  read: IOPS=10.2k, BW=160MiB/s (168MB/s)(15.6GiB/100000msec)
    slat (nsec): min=1428, max=104007, avg=1598.88, stdev=310.46
    clat (nsec): min=977, max=6907.4k, avg=95705.18, stdev=48140.11
     lat (usec): min=69, max=6909, avg=97.30, stdev=48.15
    clat percentiles (usec):
     |  1.00th=[   75],  5.00th=[   79], 10.00th=[   81], 20.00th=[   83],
     | 30.00th=[   85], 40.00th=[   86], 50.00th=[   89], 60.00th=[   91],
     | 70.00th=[   93], 80.00th=[   97], 90.00th=[  114], 95.00th=[  127],
     | 99.00th=[  223], 99.50th=[  347], 99.90th=[  750], 99.95th=[  955],
     | 99.99th=[ 1565]
   bw (  KiB/s): min=140352, max=173728, per=100.00%, avg=163916.22, stdev=6049.89, samples=199
   iops        : min= 8772, max=10858, avg=10244.77, stdev=378.13, samples=199
  lat (nsec)   : 1000=0.01%
  lat (usec)   : 20=0.01%, 50=0.01%, 100=84.55%, 250=14.64%, 500=0.52%
  lat (usec)   : 750=0.19%, 1000=0.06%
  lat (msec)   : 2=0.04%, 4=0.01%, 10=0.01%
  cpu          : usr=1.53%, sys=3.06%, ctx=1024624, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=1024023,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=160MiB/s (168MB/s), 160MiB/s-160MiB/s (168MB/s-168MB/s), io=15.6GiB (16.8GB), run=100000-100000msec

Disk stats (read/write):
  vdb: ios=1023041/0, merge=0/0, ticks=96296/0, in_queue=96296, util=99.93%
顺序写
test-job: (g=0): rw=write, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [W(1)][32.0%][w=102MiB/s][w=6542 IOPS][eta 01m:08s] 
Jobs: 1 (f=1): [W(1)][63.0%][w=99.8MiB/s][w=6390 IOPS][eta 00m:37s]
Jobs: 1 (f=1): [W(1)][94.0%][w=100MiB/s][w=6415 IOPS][eta 00m:06s]   
Jobs: 1 (f=1): [W(1)][100.0%][w=99.0MiB/s][w=6338 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2408: Thu Dec 21 13:38:25 2023
  write: IOPS=6419, BW=100MiB/s (105MB/s)(9.79GiB/100000msec); 0 zone resets
    slat (nsec): min=1465, max=84863, avg=1728.08, stdev=314.22
    clat (usec): min=56, max=86188, avg=153.70, stdev=115.95
     lat (usec): min=121, max=86190, avg=155.42, stdev=115.95
    clat percentiles (usec):
     |  1.00th=[  130],  5.00th=[  135], 10.00th=[  137], 20.00th=[  141],
     | 30.00th=[  143], 40.00th=[  145], 50.00th=[  147], 60.00th=[  149],
     | 70.00th=[  153], 80.00th=[  157], 90.00th=[  172], 95.00th=[  190],
     | 99.00th=[  277], 99.50th=[  371], 99.90th=[  709], 99.95th=[  898],
     | 99.99th=[ 1549]
   bw (  KiB/s): min=86400, max=107776, per=100.00%, avg=102740.74, stdev=2868.64, samples=199
   iops        : min= 5400, max= 6736, avg=6421.30, stdev=179.29, samples=199
  lat (usec)   : 100=0.01%, 250=98.62%, 500=1.15%, 750=0.15%, 1000=0.05%
  lat (msec)   : 2=0.03%, 4=0.01%, 10=0.01%, 100=0.01%
  cpu          : usr=0.88%, sys=2.00%, ctx=642413, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,641970,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=100MiB/s (105MB/s), 100MiB/s-100MiB/s (105MB/s-105MB/s), io=9.79GiB (10.5GB), run=100000-100000msec

Disk stats (read/write):
  vdb: ios=53/641310, merge=0/0, ticks=9/97882, in_queue=97891, util=99.93%
顺序读写
test-job: (g=0): rw=rw, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [M(1)][32.0%][r=61.1MiB/s,w=62.4MiB/s][r=3912,w=3996 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [M(1)][63.0%][r=49.1MiB/s,w=48.7MiB/s][r=3143,w=3115 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [M(1)][94.0%][r=61.4MiB/s,w=61.8MiB/s][r=3927,w=3953 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [M(1)][100.0%][r=60.8MiB/s,w=58.8MiB/s][r=3888,w=3765 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2432: Thu Dec 21 13:40:35 2023
  read: IOPS=3685, BW=57.6MiB/s (60.4MB/s)(5759MiB/100001msec)
    slat (nsec): min=1298, max=96717, avg=1440.14, stdev=343.93
    clat (nsec): min=628, max=7829.3k, avg=111727.04, stdev=181048.42
     lat (usec): min=71, max=7830, avg=113.17, stdev=181.05
    clat percentiles (usec):
     |  1.00th=[   76],  5.00th=[   80], 10.00th=[   82], 20.00th=[   84],
     | 30.00th=[   86], 40.00th=[   88], 50.00th=[   90], 60.00th=[   92],
     | 70.00th=[   96], 80.00th=[  108], 90.00th=[  126], 95.00th=[  147],
     | 99.00th=[  429], 99.50th=[  840], 99.90th=[ 2999], 99.95th=[ 3621],
     | 99.99th=[ 5800]
   bw (  KiB/s): min=46720, max=67328, per=100.00%, avg=58980.50, stdev=4631.95, samples=199
   iops        : min= 2920, max= 4208, avg=3686.28, stdev=289.50, samples=199
  write: IOPS=3680, BW=57.5MiB/s (60.3MB/s)(5750MiB/100001msec); 0 zone resets
    slat (nsec): min=1347, max=98867, avg=1643.28, stdev=365.11
    clat (usec): min=62, max=7357, avg=156.05, stdev=63.19
     lat (usec): min=122, max=7359, avg=157.69, stdev=63.20
    clat percentiles (usec):
     |  1.00th=[  130],  5.00th=[  135], 10.00th=[  137], 20.00th=[  141],
     | 30.00th=[  143], 40.00th=[  145], 50.00th=[  147], 60.00th=[  151],
     | 70.00th=[  153], 80.00th=[  157], 90.00th=[  172], 95.00th=[  194],
     | 99.00th=[  326], 99.50th=[  461], 99.90th=[  955], 99.95th=[ 1319],
     | 99.99th=[ 2245]
   bw (  KiB/s): min=46272, max=65600, per=100.00%, avg=58884.02, stdev=4635.75, samples=199
   iops        : min= 2892, max= 4100, avg=3680.25, stdev=289.73, samples=199
  lat (nsec)   : 750=0.01%
  lat (usec)   : 100=38.36%, 250=59.72%, 500=1.28%, 750=0.27%, 1000=0.10%
  lat (msec)   : 2=0.13%, 4=0.12%, 10=0.02%
  cpu          : usr=1.02%, sys=2.16%, ctx=737066, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=368589,368023,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=57.6MiB/s (60.4MB/s), 57.6MiB/s-57.6MiB/s (60.4MB/s-60.4MB/s), io=5759MiB (6039MB), run=100001-100001msec
  WRITE: bw=57.5MiB/s (60.3MB/s), 57.5MiB/s-57.5MiB/s (60.3MB/s-60.3MB/s), io=5750MiB (6030MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=368277/367607, merge=0/0, ticks=40738/56936, in_queue=97673, util=99.94%
随机
随机读
test-job: (g=0): rw=randread, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [r(1)][32.0%][r=50.7MiB/s][r=3247 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [r(1)][63.0%][r=55.0MiB/s][r=3523 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [r(1)][94.0%][r=56.4MiB/s][r=3610 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [r(1)][100.0%][r=56.1MiB/s][r=3591 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2450: Thu Dec 21 13:42:45 2023
  read: IOPS=3527, BW=55.1MiB/s (57.8MB/s)(5512MiB/100001msec)
    slat (nsec): min=1467, max=97139, avg=1650.15, stdev=341.33
    clat (usec): min=74, max=14376, avg=281.42, stdev=109.08
     lat (usec): min=75, max=14378, avg=283.07, stdev=109.08
    clat percentiles (usec):
     |  1.00th=[  186],  5.00th=[  210], 10.00th=[  223], 20.00th=[  235],
     | 30.00th=[  245], 40.00th=[  253], 50.00th=[  262], 60.00th=[  273],
     | 70.00th=[  293], 80.00th=[  310], 90.00th=[  330], 95.00th=[  379],
     | 99.00th=[  676], 99.50th=[  848], 99.90th=[ 1532], 99.95th=[ 1893],
     | 99.99th=[ 2966]
   bw (  KiB/s): min=48992, max=61504, per=100.00%, avg=56450.09, stdev=2007.10, samples=199
   iops        : min= 3062, max= 3844, avg=3528.14, stdev=125.42, samples=199
  lat (usec)   : 100=0.53%, 250=36.31%, 500=60.54%, 750=1.87%, 1000=0.46%
  lat (msec)   : 2=0.25%, 4=0.04%, 10=0.01%, 20=0.01%
  cpu          : usr=0.46%, sys=1.17%, ctx=353172, majf=0, minf=5
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=352770,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=55.1MiB/s (57.8MB/s), 55.1MiB/s-55.1MiB/s (57.8MB/s-57.8MB/s), io=5512MiB (5780MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=352443/0, merge=0/0, ticks=98827/0, in_queue=98827, util=99.93%
随机写
test-job: (g=0): rw=randwrite, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [w(1)][32.0%][w=97.3MiB/s][w=6228 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [w(1)][63.0%][w=100MiB/s][w=6402 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [w(1)][94.0%][w=98.1MiB/s][w=6280 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [w(1)][100.0%][w=99.7MiB/s][w=6379 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2463: Thu Dec 21 13:44:56 2023
  write: IOPS=6255, BW=97.7MiB/s (102MB/s)(9774MiB/100001msec); 0 zone resets
    slat (nsec): min=1493, max=23387, avg=1719.93, stdev=295.21
    clat (usec): min=118, max=17061, avg=157.71, stdev=71.22
     lat (usec): min=120, max=17063, avg=159.43, stdev=71.22
    clat percentiles (usec):
     |  1.00th=[  133],  5.00th=[  137], 10.00th=[  139], 20.00th=[  143],
     | 30.00th=[  145], 40.00th=[  147], 50.00th=[  149], 60.00th=[  151],
     | 70.00th=[  155], 80.00th=[  159], 90.00th=[  169], 95.00th=[  188],
     | 99.00th=[  355], 99.50th=[  545], 99.90th=[ 1090], 99.95th=[ 1418],
     | 99.99th=[ 2278]
   bw (  KiB/s): min=83008, max=105952, per=100.00%, avg=100109.19, stdev=4118.35, samples=199
   iops        : min= 5188, max= 6622, avg=6256.82, stdev=257.40, samples=199
  lat (usec)   : 250=98.23%, 500=1.19%, 750=0.32%, 1000=0.13%
  lat (msec)   : 2=0.11%, 4=0.01%, 10=0.01%, 20=0.01%
  cpu          : usr=0.85%, sys=2.06%, ctx=625974, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,625567,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=97.7MiB/s (102MB/s), 97.7MiB/s-97.7MiB/s (102MB/s-102MB/s), io=9774MiB (10.2GB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=91/624918, merge=0/0, ticks=17/97758, in_queue=97776, util=99.94%
随机读写
test-job: (g=0): rw=randrw, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=libaio, iodepth=1
fio-3.33
Starting 1 thread
Jobs: 1 (f=1): [m(1)][32.0%][r=32.1MiB/s,w=32.4MiB/s][r=2054,w=2073 IOPS][eta 01m:08s]
Jobs: 1 (f=1): [m(1)][63.0%][r=33.7MiB/s,w=34.0MiB/s][r=2155,w=2176 IOPS][eta 00m:37s] 
Jobs: 1 (f=1): [m(1)][94.0%][r=33.0MiB/s,w=33.5MiB/s][r=2111,w=2141 IOPS][eta 00m:06s] 
Jobs: 1 (f=1): [m(1)][100.0%][r=33.0MiB/s,w=33.8MiB/s][r=2112,w=2160 IOPS][eta 00m:00s]
test-job: (groupid=0, jobs=1): err= 0: pid=2480: Thu Dec 21 13:47:06 2023
  read: IOPS=2095, BW=32.7MiB/s (34.3MB/s)(3274MiB/100001msec)
    slat (nsec): min=1479, max=105184, avg=1672.91, stdev=370.60
    clat (usec): min=76, max=10405, avg=310.43, stdev=113.30
     lat (usec): min=78, max=10406, avg=312.10, stdev=113.30
    clat percentiles (usec):
     |  1.00th=[  215],  5.00th=[  239], 10.00th=[  249], 20.00th=[  262],
     | 30.00th=[  273], 40.00th=[  285], 50.00th=[  297], 60.00th=[  306],
     | 70.00th=[  314], 80.00th=[  326], 90.00th=[  359], 95.00th=[  424],
     | 99.00th=[  717], 99.50th=[  889], 99.90th=[ 1663], 99.95th=[ 2073],
     | 99.99th=[ 3228]
   bw (  KiB/s): min=27872, max=36224, per=100.00%, avg=33528.92, stdev=1200.94, samples=199
   iops        : min= 1742, max= 2264, avg=2095.56, stdev=75.06, samples=199
  write: IOPS=2094, BW=32.7MiB/s (34.3MB/s)(3273MiB/100001msec); 0 zone resets
    slat (nsec): min=1504, max=18916, avg=1846.38, stdev=309.15
    clat (usec): min=125, max=4810, avg=162.59, stdev=70.21
     lat (usec): min=127, max=4812, avg=164.43, stdev=70.22
    clat percentiles (usec):
     |  1.00th=[  135],  5.00th=[  139], 10.00th=[  143], 20.00th=[  145],
     | 30.00th=[  149], 40.00th=[  151], 50.00th=[  153], 60.00th=[  157],
     | 70.00th=[  159], 80.00th=[  165], 90.00th=[  174], 95.00th=[  192],
     | 99.00th=[  367], 99.50th=[  586], 99.90th=[ 1106], 99.95th=[ 1467],
     | 99.99th=[ 2147]
   bw (  KiB/s): min=28480, max=37120, per=100.00%, avg=33524.10, stdev=1493.33, samples=199
   iops        : min= 1780, max= 2320, avg=2095.26, stdev=93.33, samples=199
  lat (usec)   : 100=0.08%, 250=54.18%, 500=43.78%, 750=1.38%, 1000=0.33%
  lat (msec)   : 2=0.21%, 4=0.03%, 10=0.01%, 20=0.01%
  cpu          : usr=0.60%, sys=1.35%, ctx=419339, majf=0, minf=1
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=209544,209451,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
   READ: bw=32.7MiB/s (34.3MB/s), 32.7MiB/s-32.7MiB/s (34.3MB/s-34.3MB/s), io=3274MiB (3433MB), run=100001-100001msec
  WRITE: bw=32.7MiB/s (34.3MB/s), 32.7MiB/s-32.7MiB/s (34.3MB/s-34.3MB/s), io=3273MiB (3432MB), run=100001-100001msec

Disk stats (read/write):
  vdb: ios=209397/209259, merge=0/0, ticks=64784/33830, in_queue=98613, util=99.94%
```



```
最小0.2ms, 最大9ms, 平均0.5ms
最小0.3ms, 最大8ms, 平均0.4ms
```



