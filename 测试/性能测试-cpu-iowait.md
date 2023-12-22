

# 测试目标

10.88.201.31

# 模拟高负载

```
apt install -y stress
```

## 模拟cpu 100%

```
stress -c 8 -t 600s
```

## 模拟io

```
stress -i 2 -d 4 -t 600s
```



# 查看cpu负载情况

```
top -c
```

看第三行

例如: %Cpu(s):  0.0 us,  5.6 sy,  0.0 ni,  0.0 id, 94.4 wa,  0.0 hi,  0.0 si,  0.0 st

中的wa前的百分比





查看iowait情况(废弃)

```
apt install -y sysstat

iostat -x 1
```

# 结论

当系统盘读写压力大时，CPU会挂起, 引起系统不可用