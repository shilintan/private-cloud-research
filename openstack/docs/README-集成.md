[toc]

# openstack on kubernetes(openstack-helm)

参考文档: https://docs.openstack.org/openstack-helm/latest/

# 主机

vmware 磁盘为单一文件时服务器磁盘性能很差

系统盘为60g或者挂载一块单独的盘给容器目录

挂载1块ssd, 2块hhd

提前安装ceph



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
bash ./tools/deployment/common/prepare-k8s.sh
```

## 部署ceph(跳过)

```
cd ~/osh/openstack-helm-infra
./tools/deployment/openstack-support-rook/020-ceph.sh
bash ./tools/deployment/openstack-support-rook/025-ceph-ns-activate.sh
```

## openstack客户端

```
cd ~/osh/openstack-helm
bash ./tools/deployment/common/setup-client.sh
```

## 路由(跳过)

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/common/ingress.sh
```

## 中间件

rabbitmq, 镜像

mariadb, mysql-innodb-cluster

memcached

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/common/rabbitmq.sh
bash ./tools/deployment/component/common/mariadb.sh
bash ./tools/deployment/component/common/memcached.sh
```

​	问题: 官方提供的都是单节点的, 会有单点问题

## openstack服务

keystone -> compute-kit(placement->nova->neutron)->cinder/ceph->glance

### Keystone

身份和认证服务

身份验证和授权的中心点，管理用户身份、角色以及对 OpenStack 资源的访问

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/keystone/keystone.sh
```

### Heat

编排服务

为部署和管理云资源提供模板和自动化

将基础设施定义为代码，从而更轻松地通过模板和自动化脚本在 OpenStack 中创建和管理复杂的环境

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/heat/heat.sh
```

### Glance

镜像服务组件

管理和编目虚拟机映像，例如操作系统映像和快照

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/glance/glance.sh
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
bash ./tools/deployment/component/compute-kit/openvswitch.sh
bash ./tools/deployment/component/compute-kit/libvirt.sh
bash ./tools/deployment/component/compute-kit/compute-kit.sh
```

### Cinder

块存储服务组件

为虚拟机管理和提供持久性块存储，使用户能够根据需要将持久性存储卷附加到其虚拟机或从中分离

```
cd ~/osh/openstack-helm
bash ./tools/deployment/component/cinder/cinder.sh
```





# 调试

```
kubectl -n openstack logs -f --tail 300 mariadb-server-0
```



```
kubectl -n openstack get pod

kubectl -n openstack describe pod mariadb-ingress-error-pages-74c789984f-qp5dg
kubectl -n openstack describe pod mariadb-ingress-6764ffdb49-6cjd6
kubectl -n openstack describe pod memcached-memcached-7bdb9c5899-brwh6

kubectl -n openstack logs -f --tail 300 mariadb-ingress-5b4755745c-nh8v2

kubectl -n openstack delete pod mariadb-ingress-error-pages-869bc4b96-wqn8k
```



```
kubectl -n openstack describe pod keystone-fernet-setup-brfnc
kubectl -n openstack describe pod keystone-api-567b8d975-bnp7h
kubectl -n openstack describe pod keystone-rabbit-init-lgcbd
kubectl -n openstack describe pod keystone-domain-manage-cgmzs

```



```
kubectl -n openstack get pod|grep heat|grep -v Running|grep -v Completed
kubectl -n openstack describe pod heat-engine-7c6b5df79f-6tl7b
kubectl -n openstack describe pod heat-api-579896c87c-vpgwt
kubectl -n openstack describe pod heat-cfn-6bb5f6854c-8sxf2
kubectl -n openstack describe pod heat-engine-cleaner-28375885-qb5tc
kubectl -n openstack logs -f --tail 300 deploy/heat-engine init
kubectl -n openstack logs -f --tail 300 deploy/heat-api
kubectl -n openstack logs -f --tail 300 deploy/heat-cfn
kubectl -n openstack logs -f --tail 300 deploy/heat-engine-cleaner
```



```
kubectl -n openstack get pod|grep glance|grep -v Running|grep -v Completed
kubectl -n openstack describe pod glance-api-6bdf6749bd-vmzqx

kubectl -n  openstack get pvc
```

