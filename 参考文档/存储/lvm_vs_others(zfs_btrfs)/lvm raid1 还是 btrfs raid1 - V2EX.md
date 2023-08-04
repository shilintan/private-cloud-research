[V2EX](https://www.v2ex.com/) › [Linux](https://www.v2ex.com/go/linux)

# lvm raid1 还是 btrfs raid1

 

 [Osk](https://www.v2ex.com/member/Osk) · 2018-10-31 18:33:15 +08:00 via Android · 5339 次点击

这是一个创建于 1737 天前的主题，其中的信息可能已经有所发展或是发生改变。

NAS 上的两块机械盘，Arch Linux。

btrfs raid1 感觉好吸引人： 文件校验码 + raid 1，一旦某个盘出现问题，btrfs 能够利用校验码从 raid 镜像中取出正确的数据。

而 lvm raid1 我一直很疑惑：万一某块盘小故障，在没有报错的情况下读出的数据和另一块盘不一致，此时该以谁的为准？？？
或者万一掉电，由于磁盘品牌不一样，实际写入的数据不一样，读出来后又该以谁的为准呢？

zfs linux 的话，内存太丧病了.

btrfs raid1 的问题是：感觉 btrfs 文件系统本身比 raid0 还不稳，伤心，这么爽的文件系统，能看不敢用，btrfs raid 5/6 至于状态还不稳，可惜了。

[ Btrfs](https://www.v2ex.com/tag/Btrfs)[ raid1](https://www.v2ex.com/tag/raid1)[ raid](https://www.v2ex.com/tag/raid)[ 校验码](https://www.v2ex.com/tag/校验码)

24 条回复  **•** 2018-11-06 23:33:07 +08:00

| ![likuku](./lvm raid1 还是 btrfs raid1 - V2EX.assets/787_normal.png) |      | 1**[likuku](https://www.v2ex.com/member/likuku)**    2018-10-31 18:50:38 +08:00lvm 只是简单卷，根本不带 raid 啊，就当成是一组硬盘首尾串起来。  mdadm 倒是支持各种 raid 级别，linux 早就支持，足够成熟稳定。  传统软件 raid1 or 其它自带冗余级别 raid，还有硬件 raid 卡（好点的可以卡上配电池) 也有自己的校验方法， 无关磁盘品牌，硬盘本身控制器接口之外，I/O 都是标准化了(SATA/SAS/SCSI/IDE)。  btrfs 坑比较多(戏称 bugfs)，不如在 硬件卡 raid1，或者 mdadm 组的 raid1 之上来用 btrfs，是否跳坑自己选。  zfs linux 不堪用，那就弄个 freenas 吧，基于 freebsd，相对更成熟稳定。 freebsd 下是可以手动限制 ARC 大小，4G 物理内存下也是可以稳定跑， 开卷压缩 OK，dedup 别开，得非常强非常强的机器才带得动。  无论软硬件 raid，zfs，btrfs，备份不可少，不可少，不可少。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![Osk](./lvm raid1 还是 btrfs raid1 - V2EX.assets/102941_normal.png) |      | 2**[Osk](https://www.v2ex.com/member/Osk)**  OP  2018-10-31 20:09:55 +08:00 via Android@[likuku](https://www.v2ex.com/member/likuku) 感谢纠正！不好意思把 lvm 和 md 搞混了。其实就是担心 btrfs 不稳，如果要上 bsd 系统的话我就直接 zfs raidz 了，可惜不想用 bsd 系的，想停留在 linux 这个舒适区。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![des](./lvm raid1 还是 btrfs raid1 - V2EX.assets/88909_normal.png) |      | 3**[des](https://www.v2ex.com/member/des)**    2018-10-31 20:48:03 +08:00 via Android试试 xfs ？ |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![des](./lvm raid1 还是 btrfs raid1 - V2EX.assets/88909_normal.png) |      | 4**[des](https://www.v2ex.com/member/des)**    2018-10-31 20:49:05 +08:00 via Android@[des](https://www.v2ex.com/member/des) 应该是 raid1+xfs |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![likuku](./lvm raid1 还是 btrfs raid1 - V2EX.assets/787_normal.png) |      | 5**[likuku](https://www.v2ex.com/member/likuku)**    2018-10-31 20:51:56 +08:00@[des](https://www.v2ex.com/member/des) xfs 就只是个普通 fs 了，我自己用的电脑一直都用它（虽然很稳定坚固可靠，也有 snapshot[这功能真心没用过]，)，和 btrfs zfs lvm mdadm 之类带有 存储管理系统 职能的产品还是有相当差距，不是一个圈子的玩家。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![likuku](./lvm raid1 还是 btrfs raid1 - V2EX.assets/787_normal.png) |      | 6**[likuku](https://www.v2ex.com/member/likuku)**    2018-10-31 20:57:54 +08:00@[Osk](https://www.v2ex.com/member/Osk) 嘶... 其实嘛，bsd 才是个更为舒适的舒适区... 至少 freebsd 是这样(前提 不瞎折腾，不玩各种流行的花活)， 只用传统的职能：存储，网络，常见的网络服务。  怕断电就 UPS，错误数据回写污染什么，前几年某些大佬的观点是: “软件 RAID, 硬件 RAID 卡，几百万以下的存储设备，数据可靠性都不如带冗余的 ZFS"，那时 btrfs 还没有呢。  btrfs 坑多，慎重，无论最后选啥，独立备份不可少。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![des](./lvm raid1 还是 btrfs raid1 - V2EX.assets/88909_normal.png) |      | 7**[des](https://www.v2ex.com/member/des)**    2018-10-31 20:58:58 +08:00 via Android@[likuku](https://www.v2ex.com/member/likuku) btrfs 你都说了 bugfs，生产敢用吗？ zfs 吃内存恐怖。 lvm 不是 fs，btrfs 也不是，所以。。。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![jimzhong](./lvm raid1 还是 btrfs raid1 - V2EX.assets/97d92d618efe23b759653735df811cec.png) |      | 8**[jimzhong](https://www.v2ex.com/member/jimzhong)**    2018-10-31 21:21:57 +08:00RAID 的设计只考虑了硬盘完全故障的情况。 BRTFS 不太稳定，建议上 ZFS。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![cinhoo](./lvm raid1 还是 btrfs raid1 - V2EX.assets/f141622c1a2744719d34e7b79e56ba62.png) |      | 9**[cinhoo](https://www.v2ex.com/member/cinhoo)**    2018-10-31 21:29:15 +08:00 via iPhonebtrfs 要慎重，掉过数据 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![reus](./lvm raid1 还是 btrfs raid1 - V2EX.assets/4901_normal.png) |      | 10**[reus](https://www.v2ex.com/member/reus)**    2018-10-31 21:38:49 +08:00zfs 用了好几年，没发现有什么问题，内存占用也是在 32bit 系统才有 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![likuku](./lvm raid1 还是 btrfs raid1 - V2EX.assets/787_normal.png) |      | 11**[likuku](https://www.v2ex.com/member/likuku)**    2018-10-31 22:59:56 +08:00@[des](https://www.v2ex.com/member/des) btrfs ... 很纠结，最近一次的实践是在一台 2016 年底入的群晖 8 盘 NAS （内存加到 8G) 用的，一直尽可能保持剩余空间不低于 20%，也还好，主要是放视频素材，每日自动 snapshot，每天文件存取很频繁，一年后占用有超过 30TB，期间没出故障，奇迹一样的。一例，说服力不够，谨慎参考。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![Excalibur](./lvm raid1 还是 btrfs raid1 - V2EX.assets/20139_normal.png) |      | 12**[Excalibur](https://www.v2ex.com/member/Excalibur)**    2018-10-31 23:19:21 +08:00@[likuku](https://www.v2ex.com/member/likuku) https://www.systutorials.com/docs/linux/man/7-lvmraid/ |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![msg7086](./lvm raid1 还是 btrfs raid1 - V2EX.assets/38436_normal.png) |      | 13**[msg7086](https://www.v2ex.com/member/msg7086)**    2018-11-01 00:00:45 +08:00  ![❤️](./lvm raid1 还是 btrfs raid1 - V2EX.assets/heart_neue_red.png) 1@[likuku](https://www.v2ex.com/member/likuku) LVM 早就支持在底层隐含利用 MD 做 RAID 了。只不过比起直接用 MD 没有太大的优势。  @[Osk](https://www.v2ex.com/member/Osk) Linux 的话直接 ZFS 也是可以的，Ubuntu ZoL 呗，只要你敢用。 BugFS 谨慎！！！ MD+XFS 也是很好的选择。我 ZoL 和 MDXFS 都在用，对我来说都很稳。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![momocraft](./lvm raid1 还是 btrfs raid1 - V2EX.assets/180823_normal.png) |      | 14**[momocraft](https://www.v2ex.com/member/momocraft)**    2018-11-01 01:35:32 +08:00硬件跑 bsd, 虛擬機 linux |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![sfqtsh](./lvm raid1 还是 btrfs raid1 - V2EX.assets/127522_normal.png) |      | 15**[sfqtsh](https://www.v2ex.com/member/sfqtsh)**    2018-11-01 02:48:09 +08:00 via Android千万别用 btrfs。。。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![liuyanjun0826](./lvm raid1 还是 btrfs raid1 - V2EX.assets/72cc6deb26eed5cbee5064d3005746fa.png) |      | 16**[liuyanjun0826](https://www.v2ex.com/member/liuyanjun0826)**    2018-11-01 09:00:45 +08:00直接 fat32 raid 1，装好 windows，走到四川都不怕 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![henglinli](./lvm raid1 还是 btrfs raid1 - V2EX.assets/369137c3cdf788fd17e94606f59b8915.png) |      | 17**[henglinli](https://www.v2ex.com/member/henglinli)**    2018-11-01 10:13:50 +08:00 via iPhone人家敢把 btrfs 合进 Linux，就说明 btrfs 没问题。 btrfs 的 raid 没用过，但是普通分区下的 btrfs 从来没丢过数据。 btrfs 的 wiki 只说了它的 raid1 有性能问题 https://btrfs.wiki.kernel.org/index.php/Status#RAID1.2C_RAID10 听取别人意见时，别忘了“小马过河”这个故事。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![Osk](./lvm raid1 还是 btrfs raid1 - V2EX.assets/102941_normal.png) |      | 18**[Osk](https://www.v2ex.com/member/Osk)**  OP  2018-11-01 21:34:57 +08:00感谢楼上的劝退,,, 我最后还是选择了 lvm raid1 + xfs . 可惜没有 snapshot 功能, lvm 的 snapshot 太麻烦, 放弃. |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![henglinli](./lvm raid1 还是 btrfs raid1 - V2EX.assets/369137c3cdf788fd17e94606f59b8915.png) |      | 19**[henglinli](https://www.v2ex.com/member/henglinli)**    2018-11-02 16:21:00 +08:00 via iPhonehttps://facebookmicrosites.github.io/btrfs/docs/btrfs-docs#btrfs-in-production 在这之后发现的 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![Osk](./lvm raid1 还是 btrfs raid1 - V2EX.assets/102941_normal.png) |      | 20**[Osk](https://www.v2ex.com/member/Osk)**  OP  2018-11-04 15:15:59 +08:00 via Android@[henglinli](https://www.v2ex.com/member/henglinli) 不管了，最终我又从 lvm raid1 换成 btrfs raid1 了，lvm raid1 用初始化太头疼了，两者各有优劣，btrfs 的劣势在于可能不稳定，但快照，scrub 实在是刚需 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![henglinli](./lvm raid1 还是 btrfs raid1 - V2EX.assets/369137c3cdf788fd17e94606f59b8915.png) |      | 21**[henglinli](https://www.v2ex.com/member/henglinli)**    2018-11-04 17:24:08 +08:00 via iPhone@[Osk](https://www.v2ex.com/member/Osk) 说句得罪人的话：btrfs 自己都说了 raid1 只是不能并行时有性能问题；如果有人发现它这个功能不稳定，很有可能是这个人的问题。驾驭不了的工具，很多人会先想到是工具的问题，至于而后想到自己出问题的人就更少了，遑论首先想到自己出问题的人，经验丰富者尤甚。题外话：brtfs 前途扑朔迷离，早做好今后可能发生的迁移的准备不是坏事。 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![lucifer9](./lvm raid1 还是 btrfs raid1 - V2EX.assets/43395_normal.png) |      | 22**[lucifer9](https://www.v2ex.com/member/lucifer9)**    2018-11-04 17:45:52 +08:00btrfs 别用 raid5，6 就行 当然正常都会选择 zfs 毕竟如今笔记本都 16G 内存了 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![henglinli](./lvm raid1 还是 btrfs raid1 - V2EX.assets/369137c3cdf788fd17e94606f59b8915.png) |      | 23**[henglinli](https://www.v2ex.com/member/henglinli)**    2018-11-04 18:43:07 +08:00@[lucifer9](https://www.v2ex.com/member/lucifer9) 我没有选择 zfs 的二个非常规原因：首先我发现，即使 zfs 内核模块 bultin 了，还是得依赖 intramfs 来 import。直至半年前，zfs 任然没有内核参数来 import。issue 当然已经有人提了，不知道现在有没有 fix。后来又发现，linux 和 FreeBSD （准确点是 trueos)下创建的分区，互不能写。原因是 pool 的几个 features，不知道该怎么弄，后来就放弃了，怕弄不小心坏了旁边的 apfs。（我猜测双方会因为都认为自己默认的 feature 是最佳选择，导致这个不兼容会持续下去。） |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |

| ![likuku](./lvm raid1 还是 btrfs raid1 - V2EX.assets/787_normal.png) |      | 24**[likuku](https://www.v2ex.com/member/likuku)**    2018-11-06 23:33:07 +08:00前两天在下面这贴里看到视频，发现里面靠近结尾时，厂商工程师介绍产品时，提到是在 centos zfs 上跑 GFS：  https://www.v2ex.com/t/503875  此外，几年前查 xen 商业产品时，记得也有大厂商那时也将 ZOL 投入商用的(声称稳定性已经足以承载商用) |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
|                                                              |      |                                                              |