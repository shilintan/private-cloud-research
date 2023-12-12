# 统一测试脚本

```shell
echo 1 | bash ecs.sh
```

```shell
bash yabs.sh -i -n
```


# 测试磁盘

```
apt install -y fio

echo "顺序读"
fio -name iops -rw=read -bs=4k -runtime=10 -iodepth 1      -filename /dev/sda -ioengine libaio -direct=1
echo "随机读"
fio -name iops -rw=randread -bs=4k -runtime=10 -iodepth 1  -filename /dev/sda -ioengine libaio -direct=1
echo "顺序写"
fio -name iops -rw=write -bs=4k -runtime=10 -iodepth 1     -filename /dev/sda -ioengine libaio -direct=1
echo "随机写"
fio -name iops -rw=randwrite -bs=4k -runtime=10 -iodepth 1 -filename /dev/sda -ioengine libaio -direct=1
echo "顺序混合读写"
fio -name iops -rw=rw -bs=4k -runtime=10 -iodepth 1        -filename /dev/sda -ioengine libaio -direct=1
echo "随机混合读写"
fio -name iops -rw=randrw -bs=4k -runtime=10 -iodepth 1    -filename /dev/sda -ioengine libaio -direct=1
```


# 测试网速

```shell
wget https://wdl1.pcfg.cache.wpscdn.com/wpsdl/wpsoffice/download/12.2.0.13201/500.1001/WPSOffice_12.2.0.13201.exe
```

iperf3


# 测试带宽

```shell
yum install -y
ifconfig
iftop -n -i <网卡名>
最右边的值为带宽值使用值
```