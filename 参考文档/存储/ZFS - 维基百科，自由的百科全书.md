| **[维基百科志愿者互联交流群](https://zh.wikipedia.org/wiki/Wikipedia:中文維基百科志願者互聯交流群)**（[Telegram](https://zh.wikipedia.org/wiki/Wikipedia:TG)：[@wikipedia_zh_n](https://t.me/wikipedia_zh_n)、[Discord](https://discord.gg/77n7vnu)及[IRC](https://zh.wikipedia.org/wiki/Wikipedia:IRC)：[#wikipedia-zh](https://web.libera.chat/?chan=#wikipedia-zh) [IRC://](irc://irc.libera.chat/#wikipedia-zh)互联）欢迎大家加入。 | [[关闭](https://zh.wikipedia.org/zh-sg/ZFS#)] |
| ------------------------------------------------------------ | --------------------------------------------- |
|                                                              |                                               |

# ZFS[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=0&summary=/* top */ )]

维基百科，自由的百科全书

[跳到导航](https://zh.wikipedia.org/zh-sg/ZFS#mw-head)[跳到搜索](https://zh.wikipedia.org/zh-sg/ZFS#searchInput)

| ![img](./ZFS - 维基百科，自由的百科全书.assets/50px-Translation_to_zh_arrow.svg.png) | 此条目**可参照[英语维基百科](https://en.wikipedia.org/wiki/ZFS)相应条目来扩充**。 若您熟悉来源语言和主题，请协助[参考外语维基百科扩充条目](https://zh.wikipedia.org/wiki/Wikipedia:翻译守则)。请勿直接提交机械翻译，也不要翻译不可靠、低品质内容。依[版权协议](https://zh.wikipedia.org/wiki/Wikipedia:在维基百科内复制内容)，译文需[在编辑摘要注明来源](https://zh.wikipedia.org/wiki/Wikipedia:編輯摘要#翻譯文章)，或于讨论页顶部标记`{{Translated page}}`标签。 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

| [开发者](https://zh.wikipedia.org/wiki/软件设计师)           | [甲骨文公司](https://zh.wikipedia.org/wiki/甲骨文公司)       |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| 全称                                                         | ZFS                                                          |
| 发布                                                         | 2005年11月 ([OpenSolaris](https://zh.wikipedia.org/wiki/OpenSolaris)) |
| 结构                                                         |                                                              |
| 目录内容                                                     | 可扩展[哈希表](https://zh.wikipedia.org/wiki/哈希表)         |
| 限制                                                         |                                                              |
| 最大文件尺寸                                                 | 264字节（16 [Exabyte](https://zh.wikipedia.org/wiki/Exabyte)s) |
| 最大文件数量                                                 | 248                                                          |
| 最长文件名                                                   | 255字节                                                      |
| 最大卷容量                                                   | 264字节（16 [exabyte](https://zh.wikipedia.org/wiki/Exabyte)s） |
| 功能                                                         |                                                              |
| [岔流](https://zh.wikipedia.org/w/index.php?title=岔流_(文件系统)&action=edit&redlink=1) | 是（称作[扩展文件属性](https://zh.wikipedia.org/wiki/扩展文件属性)） |
| 属性                                                         | [POSIX](https://zh.wikipedia.org/wiki/POSIX)                 |
| [文件系统权限](https://zh.wikipedia.org/wiki/文件系统权限)   | POSIX、NFSv4 ACLs                                            |
| 透明压缩                                                     | 是                                                           |
| [透明加密](https://zh.wikipedia.org/wiki/透明加密)           | 是                                                           |
| [Data deduplication](https://zh.wikipedia.org/w/index.php?title=Data_deduplication&action=edit&redlink=1) | 是                                                           |
| [操作系统](https://zh.wikipedia.org/wiki/操作系统)支持       | [见下](https://zh.wikipedia.org/zh-sg/ZFS#对其支持的操作系统) |

**ZFS**是一个拥有[逻辑卷管理](https://zh.wikipedia.org/wiki/邏輯捲軸管理)功能的[文件系统](https://zh.wikipedia.org/wiki/檔案系統)，最早源自于[Oracle](https://zh.wikipedia.org/wiki/Oracle)为[Solaris](https://zh.wikipedia.org/wiki/Solaris)[操作系统](https://zh.wikipedia.org/wiki/操作系统)开发的文件系统。ZFS具有可扩展性，并且包括大量保护措施防止[数据损坏](https://zh.wikipedia.org/wiki/数据损坏)，支持高存储容量、高效数据压缩、集成文件系统、[卷管理](https://zh.wikipedia.org/w/index.php?title=Volume_(computing)&action=edit&redlink=1)、快照和写时复制、连续完整性检查与自动修复、[RAID-Z](https://zh.wikipedia.org/w/index.php?title=RAID-Z&action=edit&redlink=1)、原生[NFSv4](https://zh.wikipedia.org/wiki/Network_File_System#NFSv4) [ACL](https://zh.wikipedia.org/wiki/ACL)等功能，并且能被精确配置。ZFS有两个主要实现，分别来自[Oracle](https://zh.wikipedia.org/wiki/Oracle)和[OpenZFS](https://zh.wikipedia.org/wiki/OpenZFS)，它们之间极度相似，这使得ZFS在[类Unix](https://zh.wikipedia.org/wiki/Unix-like)系统中广泛可用。

ZFS这一名字本身没有含义(Zettabyte File System)，也不是某种缩写。ZFS最初是[专有软件](https://zh.wikipedia.org/wiki/专有软件)，被Sun内部开发作为Solaris的一部分，由Sun存储部门的CTO、研究员[Jeff Bonwick](https://zh.wikipedia.org/w/index.php?title=Jeff_Bonwick&action=edit&redlink=1)带领团队开发。在2005年，Solaris的大部分，包括ZFS成为采用[通用开发与散布许可证](https://zh.wikipedia.org/wiki/通用开发与散布许可证)的开源软件，作为[OpenSolaris](https://zh.wikipedia.org/wiki/OpenSolaris)项目。在2006年，ZFS成为Solaris的一项标准特性。

在2010年，Sun被Oracle收购，ZFS成为Oracle的注册商标。Oracle停止为OpenSolaris和ZFS项目提供更新的原始码，使得Oracle的ZFS转为闭源。因此，有人成立了[illumos](https://zh.wikipedia.org/wiki/Illumos)项目，去维护已经存在的开源的Solaris代码，并且在2013年成立OpenZFS以配合ZFS的开源发展。OpenZFS维护管理核心ZFS代码，而一些使用ZFS的组织维护特定的代码和ZFS所需要的验证过程，以集成到他们的系统。OpenZFS在类Unix系统中广泛使用。

## 目录



- [1历史](https://zh.wikipedia.org/zh-sg/ZFS#历史)
- [2存储池](https://zh.wikipedia.org/zh-sg/ZFS#存储池)
- [3容量](https://zh.wikipedia.org/zh-sg/ZFS#容量)
- [4写入时复制事务模型](https://zh.wikipedia.org/zh-sg/ZFS#写入时复制事务模型)
- [5快照与克隆](https://zh.wikipedia.org/zh-sg/ZFS#快照与克隆)
- [6动态条带化](https://zh.wikipedia.org/zh-sg/ZFS#动态条带化)
- [7可变块尺寸](https://zh.wikipedia.org/zh-sg/ZFS#可变块尺寸)
- [8轻量化文件系统创建](https://zh.wikipedia.org/zh-sg/ZFS#轻量化文件系统创建)
- [9储存管理](https://zh.wikipedia.org/zh-sg/ZFS#储存管理)
- [10限制](https://zh.wikipedia.org/zh-sg/ZFS#限制)
- [11专利争端](https://zh.wikipedia.org/zh-sg/ZFS#专利争端)
- [12对其支持的操作系统](https://zh.wikipedia.org/zh-sg/ZFS#对其支持的操作系统)
- [13参见](https://zh.wikipedia.org/zh-sg/ZFS#参见)
- [14参考文献](https://zh.wikipedia.org/zh-sg/ZFS#參考文獻)
- [15外部链接](https://zh.wikipedia.org/zh-sg/ZFS#外部連結)

## 历史[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=1)]

ZFS的设计与开发由Sun公司的[Jeff Bonwick](https://zh.wikipedia.org/w/index.php?title=Jeff_Bonwick&action=edit&redlink=1)所领导的一支团队完成。最早宣布于2004年9月14日，[[1\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-announce-1)于2005年10月31日并入了Solaris开发的主干原始码。[[2\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-2)并在2005年11月16日作为[OpenSolaris](https://zh.wikipedia.org/wiki/OpenSolaris) build 27的一部分发布。Sun在OpenSolaris社区开张1年后的2006年六月，将ZFS集成进了Solaris 10 6/06版本更新。[[3\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-3)

ZFS的命名来源发想于"[Zettabyte](https://zh.wikipedia.org/wiki/Zettabyte) File System"的首字母缩写。[[4\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-4)但ZFS本身并不具备任何的缩写意涵，只是作者想阐述做为一个具备高扩展容量文件系统且还有支持许多延伸功能的一个产品。

## 存储池[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=2)]

不同于传统文件系统需要驻留于单独设备或者需要一个卷管理系统去使用一个以上的设备，ZFS创建在虚拟的，被称为“zpools”的存储池之上（存储池最早在[AdvFS](https://zh.wikipedia.org/wiki/AdvFS)实现[[5\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-5)，并且加到后来的[Btrfs](https://zh.wikipedia.org/wiki/Btrfs)）。每个存储池由若干虚拟设备（*virtual devices，vdevs*）组成。这些虚拟设备可以是原始磁盘，也可能是一个[RAID1](https://zh.wikipedia.org/wiki/RAID1)镜像设备，或是非标准RAID等级的多磁盘组。于是zpool上的文件系统可以使用这些虚拟设备的总存储容量。

可以使用[磁盘限额](https://zh.wikipedia.org/w/index.php?title=磁盘限额&action=edit&redlink=1)以及设置磁盘预留空间来限制存储池中单个文件系统所占用的空间。

## 容量[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=3)]

ZFS是一个[128位](https://zh.wikipedia.org/wiki/128位)的文件系统，这意味着它能存储1800亿亿（18.4×1018）倍于当前[64位](https://zh.wikipedia.org/wiki/64位)文件系统的数据。ZFS的设计如此超前以至于这个极限就当前现实实际可能永远无法遇到。项目领导Bonwick曾说：“要填满一个128位的文件系统，将耗尽地球上所有存储设备。除非你拥有煮沸整个海洋的能量，不然你不可能将其填满。”[[1\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-announce-1)

以下是ZFS的一些理论极限：

- 248—任意文件系统的[快照](https://zh.wikipedia.org/w/index.php?title=快照&action=edit&redlink=1)数量（2×1014）
- 248—任何单独文件系统的文件数（2×1014）
- 16 [exabyte](https://zh.wikipedia.org/wiki/Exabyte)s (264 byte)—文件系统最大尺寸
- 16 exabytes (264 byte)—最大单个文件尺寸
- 16 exabytes (264 byte)—最大属性大小
- 128 [Zettabyte](https://zh.wikipedia.org/wiki/Zettabyte)s (278 byte)—最大zpool大小
- 256—单个文件的属性数量（受ZFS文件数量的约束，实际为248）
- 256—单个目录的文件数（受ZFS文件数量的约束，实际为248）
- 264—单一zpool的设备数
- 264—系统的zpools数量
- 264—单一zpool的文件系统数量

作为对这些数字的感性认识，假设每秒钟创建1,000个新文件，达到ZFS文件数极限需要大约9,000年。

在辩解填满ZFS与煮沸海洋的关系时，Bonwick写到：

> 尽管我们都希望[摩尔定律](https://zh.wikipedia.org/wiki/摩尔定律)永远延续，但是[量子力学](https://zh.wikipedia.org/wiki/量子力学)给定了任何物理设备上计算速率（computation rate）与信息量的理论极限[[来源请求\]](https://zh.wikipedia.org/wiki/Wikipedia:列明来源)。举例而言，一个质量为1[公斤](https://zh.wikipedia.org/wiki/公斤)，体积为1[升](https://zh.wikipedia.org/wiki/升)的物体，每秒至多在1031[位](https://zh.wikipedia.org/wiki/位)[信息](https://zh.wikipedia.org/wiki/信息) 上进行1051次运算[[6\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-6)。一个完全的128位存储池将包含2128个块= 2137字节= 2140位；应此，保存这些数据位至少需要(2140位) / (1031位/公斤) = 1360亿公斤的物质。

## 写入时复制事务模型[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=4)]

ZFS采用[写入时复制](https://zh.wikipedia.org/wiki/寫入時複製)事务对象模型。文件系统中的所有块指向都包含目标块的256位[校验和](https://zh.wikipedia.org/wiki/校验和)或[hash](https://zh.wikipedia.org/wiki/密码散列函数)值（目前有[Fletcher-2](https://zh.wikipedia.org/w/index.php?title=Fletcher-2&action=edit&redlink=1)、 Fletcher-4与[SHA-2](https://zh.wikipedia.org/wiki/SHA-2)供选择）[[7\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-7)。在读取块时会对这些参数加以验证。包含活动数据的块不会被覆盖，而是给修改过的数据分配一个新块，任何引用此块的[元数据](https://zh.wikipedia.org/wiki/元数据)块都被重新读取、重新分配和重写。为减少该过程的开销，多次读写更新会被归纳为一个事件组，在需要同步写入语义时会使用ZIL（[目的日志](https://zh.wikipedia.org/w/index.php?title=目的日志&action=edit&redlink=1)）写入缓存，而这些块会与校验和一同编入[Merkle 树](https://zh.wikipedia.org/w/index.php?title=Merkle_树&action=edit&redlink=1)中。

利用写入时复制使ZFS的快照和事务功能的实现变得更简单和自然，快照功能更灵活，但严重碎片化问题是其缺点之一。对于通过顺序写生成的大文件，如果以后随机的对其中的一部分进行了更改，那么这个文件在硬盘上的物理地址就变得不再连续，未来的顺序读会变得性能比较差。

## 快照与克隆[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=5)]

当ZFS写入新数据时，可以保留包含旧数据的块，因而能够维护文件系统的[快照](https://zh.wikipedia.org/wiki/快照_(電腦儲存))版本。ZFS快照具备一致性（快照基于单个时间点反映整个数据）。而因为组合快照的所有数据都会被储存，且整个存储池通常每小时会进行几次快照，所以快照的创建速度非常快。任何未变动的数据会在文件系统及其快照之间进行共享，因此也具备空间高效性。快照本质上是只读的，确保在创建后快照不会被修改。快照可以被整个恢复，也可以恢复快照中的某些文件或目录。

ZFS也可以创建可写快照（“克隆”），让两个独立的文件系统共享一组块。对克隆文件系统的修改都会创建新的数据块以反映这些更改。但是无论存在多少个克隆，未变动的块仍然会被共享。这是写入时复制原则的实施方式。

## 动态条带化[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=6)]

ZFS能动态条带化所有设备以最大化吞吐量。当额外的设备被加入到zpool中的时候，条带宽度会自动扩展以包含这些设备。这使得存储池中的所有磁盘都被用到，同时负载被平摊到所有的磁盘上。

## 可变块尺寸[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=7)]

ZFS使用可变大小的块，最大可至128KB。现有的代码允许管理员调整最大块大小，这在大块效果不好的时候有用。未来也许能做到自动调整适合工作量的块大小。[需要引用]

ZFS的可变大小的块与BtrFS和Ext4的extent不同。在ZFS中，在一个文件中所有数据块的逻辑长度必须是相同的，不同文件之间的块大小可以不同，因此ZFS可以用直接映射（direct map）的方式（同ufs/ffs/ext2/ext3）来来搜索间接块的数据指针数组（blkptr）。BtrFS和Ext4的extent方式在同一个文件中每个数据快的大小都可以不相同，因此需要用B+ Tree或者类B Tree的方式来组织间接块的数据。

虽然直接映射方式比B+ Tree的查找速度快，但是这种方式的缺点也非常明显，如：元数据开销过大、顺序IO的大文件性能不好、删除比较慢等等，因此在现代文件系统中映射方式逐渐被extent变长块取代。

如果数据压缩（LZJB）被启用，可变块大小需要被用到。如果一个数据块可被压缩至一个更小的数据块，则小的数据块将使用更少的存储和提高吞吐量（代价是增加CPU压缩和解压缩的负担）。

## 轻量化文件系统创建[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=8)]

在ZFS中，存储池中文件系统的操作相比传统文件系统的卷管理更加便捷。创建ZFS文件系统或者改变一个ZFS文件系统的大小接近于传统技术中的管理目录而非管理卷。

## 储存管理[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=9)]

## 限制[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=10)]

ZFS的最新beta版已支持透明加密。[[8\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-8)

## 专利争端[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=11)]

NetApp指控Sun的ZFS文件系统侵犯了它WAFL的七项专利，Sun[反诉](https://web.archive.org/web/20140901050937/http://news.ccidnet.com/art/1032/20071028/1255929_1.html)NetApp侵犯了12项专利，其中包括NFS协议等。后来专利争端以和解告终。

## 对其支持的操作系统[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=12)]

- [Sun Solaris](https://zh.wikipedia.org/wiki/Solaris)
- [OpenSolaris](https://zh.wikipedia.org/wiki/OpenSolaris)
- [Illumos](https://zh.wikipedia.org/wiki/Illumos)发行版
- [OpenIndiana](https://zh.wikipedia.org/wiki/OpenIndiana)
- [FreeBSD](https://zh.wikipedia.org/wiki/FreeBSD)
- [Mac OS X Server 10.5](https://zh.wikipedia.org/wiki/Mac_OS_X_Server#Mac_OS_X_Server_10.5_(Leopard_Server))
- [NetBSD](https://zh.wikipedia.org/wiki/NetBSD)
- [Linux](https://zh.wikipedia.org/wiki/Linux)（通过[用户空间文件系统](https://zh.wikipedia.org/wiki/用户空间文件系统)或原生第三方内核[可加载核心模块](https://zh.wikipedia.org/wiki/可載入核心模組)支持）[[9\]](https://zh.wikipedia.org/zh-sg/ZFS#cite_note-9)

## 参见[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=13)]

- [Btrfs](https://zh.wikipedia.org/wiki/Btrfs)
- [OpenZFS](https://zh.wikipedia.org/wiki/OpenZFS)

## 参考文献[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=14)]

1. ^ [跳转至：**1.0**](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-announce_1-0) [**1.1**](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-announce_1-1) [ZFS: the last word in file systems](http://www.sun.com/2004-0914/feature/). Sun Microsystems. September 14, 2004 [2006-04-30]. （原始内容[存档](https://web.archive.org/web/20060428092023/http://www.sun.com/2004-0914/feature/)于2006-04-28）.
2. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-2)** Jeff Bonwick. [ZFS: The Last Word in Filesystems](https://www.webcitation.org/6BNdGOIEe?url=https://blogs.oracle.com/roller-ui/errors/404.jsp). Jeff Bonwick's Blog. October 31, 2005 [2006-04-30]. （[原始内容](http://blogs.sun.com/roller/page/bonwick?entry=zfs_the_last_word_in)存档于2012-10-13）.
3. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-3)** [Sun Celebrates Successful One-Year Anniversary of OpenSolaris](http://www.sun.com/smi/Press/sunflash/2006-06/sunflash.20060620.1.xml). Sun Microsystems. June 20, 2006 [2007-04-21]. （原始内容[存档](https://web.archive.org/web/20080928001733/http://www.sun.com/smi/Press/sunflash/2006-06/sunflash.20060620.1.xml)于2008-09-28）.
4. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-4)** Jeff Bonwick. [You say zeta, I say zetta](https://www.webcitation.org/6BNdHzNju?url=https://blogs.oracle.com/roller-ui/errors/404.jsp). Jeff Bonwick's Blog. 2006-05-04 [2006-09-08]. （[原始内容](http://blogs.sun.com/bonwick/entry/you_say_zeta_i_say)存档于2012-10-13）.
5. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-5)** [AdvFS內部設計文件 (AdvFS Design Docs)](https://sourceforge.net/projects/advfs/files/AdvFS design docs/initial release/). [SourceForge.net](https://zh.wikipedia.org/wiki/SourceForge.net). [2011-01-25]. （原始内容[存档](https://web.archive.org/web/20131102181426/http://sourceforge.net/projects/advfs/files/AdvFS design docs/initial release/)于2013-11-02）.
6. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-6)** Seth Lloyd, "[Ultimate physical limits to computation（计算的终极物理限制）](http://puhep1.princeton.edu/~mcdonald/examples/QM/lloyd_nature_406_1047_00.pdf) （[页面存档备份](https://web.archive.org/web/20080807173904/http://puhep1.princeton.edu/~mcdonald/examples/QM/lloyd_nature_406_1047_00.pdf)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/互联网档案馆)）." Nature 406, 1047-1054 (2000)]
7. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-7)** [ZFS On-Disk Specification](https://web.archive.org/web/20081230170058/http://www.opensolaris.org/os/community/zfs/docs/ondiskformat0822.pdf) (PDF). Sun Microsystems, Inc. 2006 [2017年8月14日]. （[原始内容](http://opensolaris.org/os/community/zfs/docs/ondiskformat0822.pdf) (PDF)存档于2008年12月30日）. 见2.4节。
8. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-8)** [OpenSolaris Project: ZFS on disk encryption support](https://www.webcitation.org/6BNdKzJ4V?url=http://hub.opensolaris.org/bin/view/Project+zfs-crypto/WebHome). OpenSolaris Project. [2006-12-13]. （[原始内容](http://www.opensolaris.org/os/project/zfs-crypto/)存档于2012-10-13）.
9. **[^](https://zh.wikipedia.org/zh-sg/ZFS#cite_ref-9)** [1.1 What about the licensing issue?](http://zfsonlinux.org/faq.html#WhatAboutTheLicensingIssue). [November 18, 2010]. （原始内容[存档](https://web.archive.org/web/20100926104451/http://zfsonlinux.org/faq.html#WhatAboutTheLicensingIssue)于2010-09-26）.

## 外部链接[[编辑](https://zh.wikipedia.org/w/index.php?title=ZFS&action=edit&section=15)]

- [ZFS主页](https://web.archive.org/web/20070408132558/http://www.opensolaris.org/os/community/zfs/)
- [ZFS on Linux](http://www.zfsonlinux.org/) （[页面存档备份](https://web.archive.org/web/20220517072313/http://www.zfsonlinux.org/)，存于[互联网档案馆](https://zh.wikipedia.org/wiki/互联网档案馆)） - 美国[劳伦斯利福摩尔国家实验室](https://zh.wikipedia.org/wiki/勞倫斯利福摩爾國家實驗室)的ZFS on Linux开源计划

[[隐藏](https://zh.wikipedia.org/zh-sg/ZFS#)][查](https://zh.wikipedia.org/wiki/Template:文件系统)[论](https://zh.wikipedia.org/w/index.php?title=Template_talk:文件系统&action=edit&redlink=1)[编](https://zh.wikipedia.org/w/index.php?title=Template:文件系统&action=edit)[文件系统](https://zh.wikipedia.org/wiki/文件系统)[文件系统列表](https://zh.wikipedia.org/wiki/文件系统列表)[文件系统的对比](https://zh.wikipedia.org/wiki/文件系统的对比)[Unix的文件系统](https://zh.wikipedia.org/w/index.php?title=Unix的文件系统&action=edit&redlink=1)磁盘[ADFS](https://zh.wikipedia.org/w/index.php?title=高级光盘存档系统&action=edit&redlink=1)[AdvFS](https://zh.wikipedia.org/wiki/AdvFS)[Amiga FFS](https://zh.wikipedia.org/w/index.php?title=Amiga快速文件系统&action=edit&redlink=1)[Amiga OFS](https://zh.wikipedia.org/w/index.php?title=Amiga旧文件系统&action=edit&redlink=1)[APFS](https://zh.wikipedia.org/wiki/Apple文件系统)[AthFS](https://zh.wikipedia.org/w/index.php?title=AtheOS文件系统&action=edit&redlink=1)BFS [Be文件系统](https://zh.wikipedia.org/w/index.php?title=Be文件系统&action=edit&redlink=1)[启动文件系统](https://zh.wikipedia.org/w/index.php?title=启动文件系统&action=edit&redlink=1)[Btrfs](https://zh.wikipedia.org/wiki/Btrfs)[DFS](https://zh.wikipedia.org/w/index.php?title=光盘存档系统&action=edit&redlink=1)EFS [加密文件系统](https://zh.wikipedia.org/wiki/加密文件系统)[区块文件系统](https://zh.wikipedia.org/w/index.php?title=区段文件系统&action=edit&redlink=1)[Episode](https://zh.wikipedia.org/w/index.php?title=Episode文件系统&action=edit&redlink=1)[ext](https://zh.wikipedia.org/wiki/延伸檔案系統) [ext2](https://zh.wikipedia.org/wiki/Ext2)[ext3](https://zh.wikipedia.org/wiki/Ext3)[ext3cow](https://zh.wikipedia.org/w/index.php?title=Ext3cow&action=edit&redlink=1)[ext4](https://zh.wikipedia.org/wiki/Ext4)[FAT](https://zh.wikipedia.org/wiki/檔案配置表) [exFAT](https://zh.wikipedia.org/wiki/ExFAT)[Files-11](https://zh.wikipedia.org/w/index.php?title=Files-11&action=edit&redlink=1)[Fossil](https://zh.wikipedia.org/wiki/Fossil_(檔案系統))[HAMMER](https://zh.wikipedia.org/w/index.php?title=HAMMER&action=edit&redlink=1)[HFS](https://zh.wikipedia.org/wiki/分层文件系统)[HFS+](https://zh.wikipedia.org/wiki/HFS%2B)[HPFS](https://zh.wikipedia.org/wiki/高效能檔案系統)[HTFS](https://zh.wikipedia.org/w/index.php?title=高通量文件系统&action=edit&redlink=1)[IBM通用并行文件系统](https://zh.wikipedia.org/w/index.php?title=IBM通用并行文件系统&action=edit&redlink=1)[JFS](https://zh.wikipedia.org/wiki/JFS_(文件系统))[LFS](https://zh.wikipedia.org/w/index.php?title=日志结构文件系统_(BSD)&action=edit&redlink=1)MFS [Macintosh文件系统](https://zh.wikipedia.org/w/index.php?title=Macintosh文件系统&action=edit&redlink=1)[Tivo媒体文件系统](https://zh.wikipedia.org/w/index.php?title=Tivo媒体文件系统&action=edit&redlink=1)[MINIX](https://zh.wikipedia.org/wiki/MINIX文件系统)[NetWare文件系统](https://zh.wikipedia.org/w/index.php?title=NetWare文件系统&action=edit&redlink=1)[Next3](https://zh.wikipedia.org/w/index.php?title=Next3&action=edit&redlink=1)[NILFS](https://zh.wikipedia.org/w/index.php?title=NILFS&action=edit&redlink=1) [NILFS2](https://zh.wikipedia.org/w/index.php?title=NILFS2&action=edit&redlink=1)[NSS](https://zh.wikipedia.org/w/index.php?title=Novell存储服务&action=edit&redlink=1)[NTFS](https://zh.wikipedia.org/wiki/NTFS)[OneFS](https://zh.wikipedia.org/w/index.php?title=OneFS分布式文件系统&action=edit&redlink=1)[PFS](https://zh.wikipedia.org/w/index.php?title=专业文件系统&action=edit&redlink=1)[QFS](https://zh.wikipedia.org/w/index.php?title=QFS&action=edit&redlink=1)[QNX4FS](https://zh.wikipedia.org/w/index.php?title=QNX4FS&action=edit&redlink=1)[ReFS](https://zh.wikipedia.org/wiki/ReFS)[ReiserFS](https://zh.wikipedia.org/wiki/ReiserFS) [Reiser4](https://zh.wikipedia.org/w/index.php?title=Reiser4&action=edit&redlink=1)[Reliance](https://zh.wikipedia.org/w/index.php?title=Reliance_(文件系统)&action=edit&redlink=1)[Reliance Nitro](https://zh.wikipedia.org/w/index.php?title=Reliance_Nitro&action=edit&redlink=1)[RFS](https://zh.wikipedia.org/wiki/遠程文件共享)[SFS](https://zh.wikipedia.org/w/index.php?title=Smart_File_System&action=edit&redlink=1)[Soup](https://zh.wikipedia.org/w/index.php?title=Soup_(苹果公司)&action=edit&redlink=1)[Tux3](https://zh.wikipedia.org/w/index.php?title=Tux3&action=edit&redlink=1)[UBIFS](https://zh.wikipedia.org/wiki/UBIFS)[UFS](https://zh.wikipedia.org/wiki/Unix文件系统)[VxFS](https://zh.wikipedia.org/w/index.php?title=VxFS&action=edit&redlink=1)[WAFL](https://zh.wikipedia.org/w/index.php?title=任意位置写入文件布局&action=edit&redlink=1)[Xiafs](https://zh.wikipedia.org/w/index.php?title=Xiafs&action=edit&redlink=1)[XFS](https://zh.wikipedia.org/wiki/XFS)[Xsan](https://zh.wikipedia.org/w/index.php?title=Xsan&action=edit&redlink=1)[zFS](https://zh.wikipedia.org/w/index.php?title=ZFS_(z/OS_file_system)&action=edit&redlink=1)ZFS[光盘](https://zh.wikipedia.org/wiki/光碟)[HSF](https://zh.wikipedia.org/w/index.php?title=HSF&action=edit&redlink=1)[ISO 9660](https://zh.wikipedia.org/wiki/ISO_9660)[ISO 13490](https://zh.wikipedia.org/w/index.php?title=ISO_13490&action=edit&redlink=1)[UDF](https://zh.wikipedia.org/wiki/通用光碟格式)[闪存](https://zh.wikipedia.org/wiki/闪存)和[SSD](https://zh.wikipedia.org/wiki/固态硬盘)[APFS](https://zh.wikipedia.org/wiki/Apple文件系统)[FAT](https://zh.wikipedia.org/wiki/檔案配置表)[exFAT](https://zh.wikipedia.org/wiki/ExFAT)[CHFS](https://zh.wikipedia.org/w/index.php?title=CHFS&action=edit&redlink=1)[TFAT](https://zh.wikipedia.org/wiki/事务安全FAT文件系统)[EROFS](https://zh.wikipedia.org/w/index.php?title=EROFS&action=edit&redlink=1)[FFS2](https://zh.wikipedia.org/wiki/快閃記憶體檔案系統)[F2FS](https://zh.wikipedia.org/wiki/F2FS)[HPFS](https://zh.wikipedia.org/wiki/高效能檔案系統)[JFFS](https://zh.wikipedia.org/w/index.php?title=JFFS&action=edit&redlink=1)[JFFS2](https://zh.wikipedia.org/wiki/JFFS2)[JFS](https://zh.wikipedia.org/wiki/JFS_(文件系统))[LogFS](https://zh.wikipedia.org/wiki/LogFS)[NILFS](https://zh.wikipedia.org/w/index.php?title=NILFS&action=edit&redlink=1) [NILFS2](https://zh.wikipedia.org/w/index.php?title=NILFS2&action=edit&redlink=1)[NVFS](https://zh.wikipedia.org/w/index.php?title=非易失性文件系统&action=edit&redlink=1)[YAFFS](https://zh.wikipedia.org/wiki/YAFFS)[UBIFS](https://zh.wikipedia.org/wiki/UBIFS)[分布式](https://zh.wikipedia.org/wiki/集群文件系统)[CXFS](https://zh.wikipedia.org/w/index.php?title=CXFS&action=edit&redlink=1)[GFS2](https://zh.wikipedia.org/w/index.php?title=GFS2&action=edit&redlink=1)[Google文件系统](https://zh.wikipedia.org/wiki/Google檔案系統)[OCFS2](https://zh.wikipedia.org/w/index.php?title=OCFS2&action=edit&redlink=1)[OrangeFS](https://zh.wikipedia.org/w/index.php?title=OrangeFS&action=edit&redlink=1)[PVFS](https://zh.wikipedia.org/w/index.php?title=PVFS&action=edit&redlink=1)[QFS](https://zh.wikipedia.org/w/index.php?title=QFS&action=edit&redlink=1)[Xsan](https://zh.wikipedia.org/w/index.php?title=Xsan&action=edit&redlink=1)*[更多...](https://zh.wikipedia.org/wiki/文件系统列表#分布式文件系统)*[NAS](https://zh.wikipedia.org/wiki/網路附加儲存)[AFS](https://zh.wikipedia.org/wiki/安德魯檔案系統)（[OpenAFS](https://zh.wikipedia.org/wiki/OpenAFS)）[AFP](https://zh.wikipedia.org/wiki/苹果归档协议)[Coda](https://zh.wikipedia.org/wiki/Coda)[DFS](https://zh.wikipedia.org/wiki/分散式檔案系統_(Microsoft))[GPFS](https://zh.wikipedia.org/w/index.php?title=通用并行文件系统&action=edit&redlink=1)[Google文件系统](https://zh.wikipedia.org/wiki/Google檔案系統)[Lustre](https://zh.wikipedia.org/wiki/Lustre)[NCP](https://zh.wikipedia.org/wiki/NetWare核心協定)[NFS](https://zh.wikipedia.org/wiki/网络文件系统)[POHMELFS](https://zh.wikipedia.org/w/index.php?title=POHMELFS&action=edit&redlink=1)[Hadoop](https://zh.wikipedia.org/wiki/Apache_Hadoop)[SMB (CIFS)](https://zh.wikipedia.org/wiki/伺服器訊息區塊)[SSHFS](https://zh.wikipedia.org/wiki/SSHFS)*[更多...](https://zh.wikipedia.org/wiki/文件系统列表)*特殊[Aufs](https://zh.wikipedia.org/wiki/Aufs)[AXFS](https://zh.wikipedia.org/w/index.php?title=AXFS&action=edit&redlink=1)[启动文件系统](https://zh.wikipedia.org/w/index.php?title=启动文件系统&action=edit&redlink=1)[CDfs](https://zh.wikipedia.org/w/index.php?title=CDfs&action=edit&redlink=1)[光盘文件系统](https://zh.wikipedia.org/w/index.php?title=光盘文件系统&action=edit&redlink=1)[Cramfs](https://zh.wikipedia.org/wiki/Cramfs)[Davfs2](https://zh.wikipedia.org/w/index.php?title=Davfs2&action=edit&redlink=1)[EROFS](https://zh.wikipedia.org/w/index.php?title=EROFS&action=edit&redlink=1)[FTPFS](https://zh.wikipedia.org/wiki/FTPFS)[FUSE](https://zh.wikipedia.org/wiki/FUSE)[GmailFS](https://zh.wikipedia.org/wiki/GmailFS)[Lnfs](https://zh.wikipedia.org/wiki/Lnfs)[LTFS](https://zh.wikipedia.org/w/index.php?title=线性磁带文件系统&action=edit&redlink=1)[MVFS](https://zh.wikipedia.org/w/index.php?title=Rational_MultiVersion_File_System&action=edit&redlink=1)[SquashFS](https://zh.wikipedia.org/wiki/SquashFS)[UMSDOS](https://zh.wikipedia.org/w/index.php?title=FAT文件系统与Linux&action=edit&redlink=1)[OverlayFS](https://zh.wikipedia.org/wiki/OverlayFS)[UnionFS](https://zh.wikipedia.org/w/index.php?title=UnionFS&action=edit&redlink=1)[WBFS](https://zh.wikipedia.org/w/index.php?title=WBFS&action=edit&redlink=1)伪[configfs](https://zh.wikipedia.org/w/index.php?title=Configfs&action=edit&redlink=1)[devfs](https://zh.wikipedia.org/w/index.php?title=设备文件_(Unix)&action=edit&redlink=1)[debugfs](https://zh.wikipedia.org/w/index.php?title=Debugfs&action=edit&redlink=1)[kernfs](https://zh.wikipedia.org/w/index.php?title=Kernfs&action=edit&redlink=1)[procfs](https://zh.wikipedia.org/wiki/Procfs)[specfs](https://zh.wikipedia.org/wiki/Specfs)[sysfs](https://zh.wikipedia.org/wiki/Sysfs)[tmpfs](https://zh.wikipedia.org/wiki/Tmpfs)[WinFS](https://zh.wikipedia.org/wiki/WinFS)[加密](https://zh.wikipedia.org/w/index.php?title=文件系统级加密&action=edit&redlink=1)[eCryptfs](https://zh.wikipedia.org/wiki/ECryptfs)[EncFS](https://zh.wikipedia.org/wiki/EncFS)[EFS](https://zh.wikipedia.org/wiki/加密文件系统)[Rubberhose](https://zh.wikipedia.org/w/index.php?title=Rubberhose&action=edit&redlink=1)[SSHFS](https://zh.wikipedia.org/wiki/SSHFS)ZFS类型[集群](https://zh.wikipedia.org/wiki/集群文件系统) [全局](https://zh.wikipedia.org/w/index.php?title=全局文件系统&action=edit&redlink=1)[网格](https://zh.wikipedia.org/w/index.php?title=网格文件系统&action=edit&redlink=1)[自我认证](https://zh.wikipedia.org/w/index.php?title=自我认证文件系统&action=edit&redlink=1)[闪存](https://zh.wikipedia.org/wiki/快閃記憶體檔案系統)[日志](https://zh.wikipedia.org/wiki/日志文件系统)[日志结构](https://zh.wikipedia.org/w/index.php?title=日志结构文件系统&action=edit&redlink=1)[对象](https://zh.wikipedia.org/wiki/对象存储)[面向记录](https://zh.wikipedia.org/w/index.php?title=面向记录文件系统&action=edit&redlink=1)[语义](https://zh.wikipedia.org/w/index.php?title=语义文件系统&action=edit&redlink=1)[隐写](https://zh.wikipedia.org/w/index.php?title=隐写文件系统&action=edit&redlink=1)[合成](https://zh.wikipedia.org/w/index.php?title=合成文件系统&action=edit&redlink=1)[版本](https://zh.wikipedia.org/w/index.php?title=版本文件系统&action=edit&redlink=1)特性[保留大小写](https://zh.wikipedia.org/wiki/保留大小写)[写入时复制](https://zh.wikipedia.org/wiki/寫入時複製)[重复数据删除](https://zh.wikipedia.org/wiki/重复数据删除)[数据擦洗](https://zh.wikipedia.org/w/index.php?title=数据擦洗&action=edit&redlink=1)[原地执行](https://zh.wikipedia.org/w/index.php?title=原地执行&action=edit&redlink=1)[Extent](https://zh.wikipedia.org/wiki/Extent_(檔案系統))[文件属性](https://zh.wikipedia.org/wiki/文件属性) [扩展文件属性](https://zh.wikipedia.org/wiki/扩展文件属性)[文件更改日志](https://zh.wikipedia.org/wiki/文件更改日志)[Fork](https://zh.wikipedia.org/wiki/Fork_(文件系统))链接 [硬链接](https://zh.wikipedia.org/wiki/硬链接)[符号链接](https://zh.wikipedia.org/wiki/符号链接)[访问控制](https://zh.wikipedia.org/w/index.php?title=计算机访问控制&action=edit&redlink=1)[访问控制表](https://zh.wikipedia.org/wiki/存取控制串列)[文件系统级加密](https://zh.wikipedia.org/w/index.php?title=文件系统级加密&action=edit&redlink=1)[权限](https://zh.wikipedia.org/wiki/文件系统权限) [Modes](https://zh.wikipedia.org/w/index.php?title=Modes_(Unix)&action=edit&redlink=1)[粘滞位](https://zh.wikipedia.org/wiki/粘滞位)[接口](https://zh.wikipedia.org/wiki/介面_(資訊科技))[文件管理器](https://zh.wikipedia.org/wiki/檔案管理器)[文件系统API](https://zh.wikipedia.org/w/index.php?title=文件系统API&action=edit&redlink=1) [可安装文件系统](https://zh.wikipedia.org/wiki/可安装文件系统)[虚拟文件系统](https://zh.wikipedia.org/wiki/虛擬檔案系統)

[分类](https://zh.wikipedia.org/wiki/Special:页面分类)：

- [磁盘文件系统](https://zh.wikipedia.org/wiki/Category:磁盘文件系统)
- [太阳微系统软件](https://zh.wikipedia.org/wiki/Category:昇陽電腦軟體)
- [Linux档案系统](https://zh.wikipedia.org/wiki/Category:Linux檔案系統)
- [OpenSolaris](https://zh.wikipedia.org/wiki/Category:OpenSolaris)