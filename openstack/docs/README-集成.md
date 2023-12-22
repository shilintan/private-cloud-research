[toc]

# openstack on kubernetes(openstack-helm)

参考文档: https://docs.openstack.org/openstack-helm/latest/

# 主机

vmware 磁盘为单一文件时服务器磁盘性能很差

系统盘为60g或者挂载一块单独的盘给容器目录

挂载1块ssd, 2块hhd

提前安装ceph



```
# 检查虚拟化情况
# 如果不是none, nove需要使用libvirt
systemd-detect-virt
```



## 准备脚本文件

## 自定义

```
上传文件 deploy.zip

apt install -y unzip
apt install -y dos2unix
apt install -y make
apt install -y python3-pip
apt install -y jq
apt install -y bc
apt install -y vim


rm /usr/lib/python3.11/EXTERNALLY-MANAGED

rm -rf ~/osh
mkdir ~/osh
cp -f ~/deploy.zip ~/osh
cd ~/osh
rm -rf ~/osh/openstack-helm ~/osh/openstack-helm-infra
unzip deploy.zip


find . -type f -print0 | xargs -0 dos2unix
find . -type f -print0 | xargs -0 chmod +x

export OPENSTACK_RELEASE=2023.2
export CONTAINER_DISTRO_NAME=ubuntu
export CONTAINER_DISTRO_VERSION=jammy
```



废弃

```
kubectl -n openstack get svc

cat >> /etc/hosts <<EOF
10.96.2.163 keystone-api.openstack.svc.cluster.local
heat.openstack.svc.cluster.local
EOF
```







## 官方(废弃)

```
mkdir ~/osh
cd ~/osh
git clone https://opendev.org/openstack/openstack-helm.git
git clone https://opendev.org/openstack/openstack-helm-infra.git


export OPENSTACK_RELEASE=2023.2
export CONTAINER_DISTRO_NAME=ubuntu
export CONTAINER_DISTRO_VERSION=jammy
```

## 主机标记、命名空间

```
cd ~/osh/openstack-helm
./tools/deployment/common/prepare-k8s.sh
```

## 部署ceph(跳过)

```
cd ~/osh/openstack-helm-infra
./tools/deployment/openstack-support-rook/020-ceph.sh
bash ./tools/deployment/openstack-support-rook/025-ceph-ns-activate.sh
```

## openstack客户端

配置淘宝源

```
mkdir -p ~/.pip
cat >> ~/.pip/pip.conf <<EOF
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
EOF
```



```
cd ~/osh/openstack-helm
./tools/deployment/common/setup-client.sh
```

## 路由(跳过)

```
cd ~/osh/openstack-helm
./tools/deployment/component/common/ingress.sh
```

## 中间件

rabbitmq, 镜像

mariadb, mysql-innodb-cluster

memcached

```
cd ~/osh/openstack-helm
./tools/deployment/component/common/rabbitmq.sh
./tools/deployment/component/common/mariadb.sh
./tools/deployment/component/common/memcached.sh
```

​	问题: 官方提供的都是单节点的, 会有单点问题

## openstack服务

keystone -> compute-kit(placement->nova->neutron)->cinder/ceph->glance

### Keystone

身份和认证服务

身份验证和授权的中心点，管理用户身份、角色以及对 OpenStack 资源的访问

```
cd ~/osh/openstack-helm
./tools/deployment/component/keystone/keystone.sh
```

### Heat

编排服务

为部署和管理云资源提供模板和自动化

将基础设施定义为代码，从而更轻松地通过模板和自动化脚本在 OpenStack 中创建和管理复杂的环境

```
cd ~/osh/openstack-helm
./tools/deployment/component/heat/heat.sh
```

### Glance

镜像服务组件

管理和编目虚拟机映像，例如操作系统映像和快照

```
cd ~/osh/openstack-helm
./tools/deployment/component/glance/glance.sh
```

### compute kit

Placement

​	帮助管理和分配 OpenStack 云环境中资源的服务

​	帮助 Nova（计算）为虚拟机实例找到并分配正确的资源（CPU、内存等）

Nova

​	管理和编排 OpenStack 云中虚拟机的计算服务

​	配置和调度实例，处理它们的生命周期，并与底层虚拟机管理程序交互

Neutron

​	网络服务

​	提供网络连接并使用户能够为其虚拟机和其他服务创建和管理网络资源

​	OpenVSwitch比linuxbridge 更适用于大规模复杂场景(多机房)



```
cd ~/osh/openstack-helm
./tools/deployment/component/compute-kit/openvswitch.sh
./tools/deployment/component/compute-kit/libvirt.sh
./tools/deployment/component/compute-kit/compute-kit.sh
```

### Cinder

块存储服务组件

为虚拟机管理和提供持久性块存储，使用户能够根据需要将持久性存储卷附加到其虚拟机或从中分离

```
cd ~/osh/openstack-helm
./tools/deployment/component/cinder/cinder.sh
```





# 调试

```
kubectl -n openstack get pod|grep -v Completed|grep -v Running
kubectl -n openstack get pvc
```



```
kubectl -n openstack logs -f --tail 300 mariadb-server-0
```

rabbitmq

```
kubectl -n openstack get pvc|grep rabbitmq
kubectl -n openstack get svc|grep rabbitmq
kubectl -n openstack get pod|grep rabbitmq
kubectl -n openstack describe pod rabbitmq-rabbitmq-0
kubectl -n openstack describe pvc rabbitmq-data-rabbitmq-rabbitmq-0
kubectl -n openstack describe pod rabbitmq-cluster-wait-h4fh6
kubectl -n openstack logs -f --tail 300 rabbitmq-rabbitmq-0
kubectl -n openstack logs -f --tail 300 rabbitmq-rabbitmq-1
kubectl -n openstack delete pvc rabbitmq-data-rabbitmq-rabbitmq-0
kubectl -n openstack describe pod glance-api-77ff8d866b-rw2w4
kubectl -n openstack describe pod placement-api-5d59ddbdf4-thq5x
kubectl -n openstack describe pod nova-bootstrap-5fckx
kubectl -n openstack describe pod neutron-metadata-agent-default-gjmgw
kubectl -n openstack edit pod rabbitmq-rabbitmq-0
kubectl -n openstack edit svc rabbitmq
kubectl -n openstack exec -it rabbitmq-rabbitmq-0 -- bash
	rabbitmqctl list_users
	rabbitmqctl list_connections
	rabbitmqctl	list_queues
	rabbitmqctl	list_exchanges
	rabbitmqctl list_consumers
	rabbitmqctl list_consumers compute
```

mariadb

```
kubectl -n openstack get pod|grep mariadb
kubectl -n openstack describe pod mariadb-ingress-error-pages-74c789984f-qp5dg
kubectl -n openstack describe pod mariadb-ingress-6764ffdb49-6cjd6
kubectl -n openstack describe pod memcached-memcached-7bdb9c5899-brwh6

kubectl -n openstack logs -f --tail 300 mariadb-ingress-5b4755745c-nh8v2
kubectl -n openstack logs -f --tail 300 mariadb-server-0

kubectl -n openstack delete pod mariadb-ingress-error-pages-869bc4b96-wqn8k
```

memcached

```
kubectl -n openstack get pod|grep memcached
kubectl -n openstack logs -f --tail 300 deploy/memcached-memcached
```

keystone

```
kubectl -n openstack get pod|grep keystone

kubectl -n openstack describe pod keystone-fernet-setup-brfnc
kubectl -n openstack describe pod keystone-api-567b8d975-bnp7h
kubectl -n openstack describe pod keystone-rabbit-init-lgcbd
kubectl -n openstack describe pod keystone-domain-manage-cgmzs
kubectl -n openstack edit pod keystone-api-567b8d975-kltn5

kubectl -n openstack describe pod -l application=keystone
kubectl -n openstack logs -f --tail 300 deploy/keystone-api
```

heat

```
kubectl -n openstack get pod|grep heat
kubectl -n openstack get pod|grep heat|grep -v Running|grep -v Completed
kubectl -n openstack describe pod heat-engine-7c6b5df79f-6tl7b
kubectl -n openstack describe pod heat-api-579896c87c-vpgwt
kubectl -n openstack describe pod heat-cfn-6bb5f6854c-8sxf2
kubectl -n openstack describe pod heat-engine-cleaner-28375885-qb5tc
kubectl -n openstack logs -f --tail 300 deploy/heat-engine init
kubectl -n openstack logs -f --tail 300 deploy/heat-api
kubectl -n openstack logs -f --tail 300 deploy/heat-cfn
kubectl -n openstack logs -f --tail 300 deploy/heat-engine-cleaner

kubectl -n openstack describe pod -l application=heat,component=api
kubectl -n openstack describe pod -l application=heat,component=cfn
kubectl -n openstack describe pod -l application=heat,component=engine


kubectl -n openstack edit pod heat-engine-8666598c6d-sgp92
```

glance

```
kubectl -n openstack get pvc
kubectl -n openstack get pod|grep glance
kubectl -n openstack get pod|grep glance|grep -v Running|grep -v Completed
kubectl -n openstack describe pod -l application=glance,component=api
kubectl -n openstack logs -f --tail 300 deploy/glance-api
kubectl -n openstack logs -f --tail 300 glance-bootstrap-rjn2v
kubectl -n openstack edit pod glance-api-6ddd676c9f-tpnj7

```

openvswitch

```
kubectl -n openstack get pod|grep openvswitch
kubectl -n openstack logs -f --tail 300 openvswitch-48bss
kubectl -n openstack logs -f --tail 300 neutron-ovs-agent-default-rmbgw
```

libvirt

```
kubectl -n openstack get pod|grep libvirt
kubectl -n openstack logs -f --tail 300 libvirt-libvirt-default-846ns init
kubectl -n openstack logs -f --tail 300 libvirt-libvirt-default-846ns
kubectl -n openstack describe pod -l application=libvirt
kubectl -n openstack edit pod libvirt-libvirt-default-846ns
```

placement

```
kubectl -n openstack get pod|grep placement

kubectl -n openstack describe pod -l application=placement,component=ks-service
kubectl -n openstack describe pod -l application=placement,component=api
kubectl -n openstack logs -f --tail 300 deploy/placement-api
kubectl -n openstack edit pod placement-api-5b7b8d9ddb-wtwmh
```

nova

```
kubectl -n openstack get pod|grep nova
kubectl -n openstack get pod|grep nova|grep -v Completed
kubectl -n openstack get pod|grep nova|grep -v Completed|grep -v 1/1
kubectl -n openstack get pod -l application=nova,component=compute
kubectl -n openstack describe pod -l application=nova,component=metadata
kubectl -n openstack describe pod -l application=nova,component=cell-setup
kubectl -n openstack describe pod -l application=nova,component=compute
kubectl -n openstack describe pod nova-conductor-648c755b75-p99tn
kubectl -n openstack logs -f --tail 300 deploy/nova-api-osapi
kubectl -n openstack logs -f --tail 300 deploy/nova-api-metadata
kubectl -n openstack logs -f --tail 300 deploy/nova-conductor
kubectl -n openstack logs -f --tail 300 deploy/nova-novncproxy
kubectl -n openstack logs -f --tail 300 deploy/nova-scheduler
kubectl -n openstack logs -f --tail 300 nova-cell-setup-z58xq init
kubectl -n openstack logs -f --tail 300 nova-cell-setup-28382580-grqqf init
kubectl -n openstack logs -f --tail 300 nova-compute-default-4k8lb

kubectl -n openstack edit pod nova-cell-setup-28382580-grqqf
kubectl -n openstack exec -it nova-compute-default-4k8lb -- bash
	cat /etc/nova/nova.conf|grep rabbitmq
	python /tmp/health-probe.py	--config-file /etc/nova/nova.conf --service-queue-name compute --use-fqdn
	python /tmp/health-probe.py	--config-file /etc/nova/nova.conf --service-queue-name compute --liveness-probe --use-fqdn

kubectl -n openstack delete pod nova-cell-setup-9c46p
kubectl -n openstack delete pod -l component=compute
```

neutron

```
kubectl -n openstack get svc|grep neutron
kubectl -n openstack get endpoints|grep neutron
kubectl -n openstack get pod|grep neutron|grep -v Completed
kubectl -n openstack get pod|grep neutron|grep -v Completed|grep -v 1/1
kubectl -n openstack get pod -l application=neutron,component=neutron-ovs-agent
kubectl -n openstack describe pod neutron-metadata-agent-default-kdl2k
kubectl -n openstack logs -f --tail 300 neutron-metadata-agent-default-kdl2k init
kubectl -n openstack delete pod neutron-metadata-agent-default-kdl2k

kubectl -n openstack logs -f --tail 300 deploy/neutron-server
kubectl -n openstack logs -f --tail 300 neutron-ovs-agent-default-rmbgw
kubectl -n openstack logs -f --tail 300 neutron-netns-cleanup-cron-default-fzvjh
kubectl -n openstack logs -f --tail 300 neutron-l3-agent-default-xmd7t
kubectl -n openstack logs -f --tail 300 neutron-dhcp-agent-default-dp52b neutron-dhcp-agent-init
kubectl -n openstack logs -f --tail 300 neutron-metadata-agent-default-jh8w5 init

kubectl -n openstack describe pod neutron-metadata-agent-default-jh8w5
kubectl -n openstack describe pod neutron-l3-agent-default-xmd7t
	
kubectl -n openstack edit pod neutron-ovs-agent-default-9cnxj

kubectl -n openstack exec -it neutron-dhcp-agent-default-dp52b -- bash
	python /tmp/health-probe.py --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/dhcp_agent.ini --agent-queue-name dhcp_agent --use-fqdn
```



```
kubectl -n openstack get pod|grep nova
kubectl -n openstack get pod|grep nova

kubectl -n openstack get endpoints |grep rabbitmq
kubectl -n openstack get endpoints |grep neutron-server
kubectl -n openstack get endpoints |grep nova-api
kubectl -n openstack get endpoints |grep metadata

kubectl -n openstack edit svc metadata
```



## 清理

```
helm delete -n openstack rabbitmq
kubectl -n openstack delete pvc rabbitmq-data-rabbitmq-rabbitmq-0

helm delete -n openstack mariadb
helm delete -n openstack glance
helm delete -n openstack nova
helm delete -n openstack neutron

kubectl -n openstack delete pvc rabbitmq-data-rabbitmq-rabbitmq-0
kubectl -n openstack delete pvc rabbitmq-data-rabbitmq-rabbitmq-0
kubectl -n openstack delete pvc glance-images

```

# 访问

rabbitmq: http://192.168.203.174:32419	rabbitmq/password

# 问题

nova 无法连接rabbitmq问题, 可能是rabbitmq性能过低, 换成本地目录测试一下

