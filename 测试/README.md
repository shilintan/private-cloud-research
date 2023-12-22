# 统一测试脚本

```shell
echo 1 | bash ecs.sh
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