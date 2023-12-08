# 目标

创建虚拟机, 创建windows虚拟机

# Openstack是什么

虚拟机级别虚拟化平台

开源的

![img](./arch/openstack-map-v20230501.png)





**Compute**

NOVA		[Compute Service](https://www.openstack.org/software/releases/antelope/components/nova)

ZUN		[Containers Service](https://www.openstack.org/software/releases/antelope/components/zun)

**Hardware Lifecycle**

IRONIC		[Bare Metal Provisioning Service](https://www.openstack.org/software/releases/antelope/components/ironic)

CYBORG		[Lifecycle management of accelerators](https://www.openstack.org/software/releases/antelope/components/cyborg)

**Storage**

SWIFT		[Object store](https://www.openstack.org/software/releases/antelope/components/swift)

CINDER		[Block Storage](https://www.openstack.org/software/releases/antelope/components/cinder)

MANILA		[Shared filesystems](https://www.openstack.org/software/releases/antelope/components/manila)

**Networking**

NEUTRON		[Networking](https://www.openstack.org/software/releases/antelope/components/neutron)

OCTAVIA		[Load balancer](https://www.openstack.org/software/releases/antelope/components/octavia)

DESIGNATE		[DNS service](https://www.openstack.org/software/releases/antelope/components/designate)

**Shared Services**

KEYSTONE		[Identity service](https://www.openstack.org/software/releases/antelope/components/keystone)

PLACEMENT		[Placement service](https://www.openstack.org/software/releases/antelope/components/placement)

GLANCE		[Image service](https://www.openstack.org/software/releases/antelope/components/glance)

BARBICAN		[Key management](https://www.openstack.org/software/releases/antelope/components/barbican)

**Orchestration**

HEAT		[Orchestration](https://www.openstack.org/software/releases/antelope/components/heat)

SENLIN		[Clustering service](https://www.openstack.org/software/releases/antelope/components/senlin)

MISTRAL			[Workflow service](https://www.openstack.org/software/releases/antelope/components/mistral)

ZAQAR

[Messaging Service](https://www.openstack.org/software/releases/antelope/components/zaqar)

BLAZAR

[Resource reservation service](https://www.openstack.org/software/releases/antelope/components/blazar)

AODH

[Alarming Service](https://www.openstack.org/software/releases/antelope/components/aodh)

**Workload Provisioning**

MAGNUM

[Container Orchestration Engine Provisioning](https://www.openstack.org/software/releases/antelope/components/magnum)

SAHARA

[Big Data Processing Framework Provisioning](https://www.openstack.org/software/releases/antelope/components/sahara)

TROVE

[Database as a Service](https://www.openstack.org/software/releases/antelope/components/trove)

**Application Lifecycle**

MASAKARI

[Instances High Availability Service](https://www.openstack.org/software/releases/antelope/components/masakari)

MURANO

[Application Catalog](https://www.openstack.org/software/releases/antelope/components/murano)

SOLUM

[Software Development Lifecycle Automation](https://www.openstack.org/software/releases/antelope/components/solum)

FREEZER

[Backup, Restore, and Disaster Recovery](https://www.openstack.org/software/releases/antelope/components/freezer)

**API Proxies**

EC2API

[EC2 API proxy](https://www.openstack.org/software/releases/antelope/components/ec2api)

**Web frontends**

HORIZON

[Dashboard](https://www.openstack.org/software/releases/antelope/components/horizon)

SKYLINE

[Next generation dashboard (emerging technology)](https://www.openstack.org/software/releases/antelope/components/skyline)

## 核心

计算: Nova、Zun

网络: Neutron



# 下载-安装操作系统

ubuntu lts

​	https://releases.ubuntu.com



配置镜像代理源: http://mirrors.aliyun.com/ubuntu/

# 本地尝试

devstack

​	https://docs.openstack.org/devstack/latest/#add-stack-user-optional



# 版本列表

https://www.openstack.org/software/roadmap/

yoga

# 计算

基于KVM