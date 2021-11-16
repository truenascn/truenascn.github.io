# TrueNAS Core和TrueNAS Enterprise

![](https://www.truenas.com/docs/images/tn-openstorage-logo.png)

TrueNAS是目前世界上最受欢迎的开源存储管理系统，同时也是目前通过网络管理和分享数据的最有效的解决方案。TrueNAS也是为您的数据创建一个安全、可靠、集中且易于访问的场所的最简单方法。TrueNAS Open Storage 为文件、块、对象和应用程序数据提供统一存储。

TrueNAS 几乎可以安装在任何硬件平台上，适用于家庭、中小型企业和大型企业应用程序。 TrueNAS 共有三个版本，可支持广泛的应用程序，同时共享通用管理工具并支持数据传输：

| ![](https://www.truenas.com/docs/images/tn-core-logo.png)    | ![](https://www.truenas.com/docs/images/tn-enterprise-logo.png) | ![](https://www.truenas.com/docs/images/tn-scale-logo.png)   |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **TrueNAS CORE** 是免费和开源的，是广受欢迎的 FreeNAS 的继承者。 它几乎可以在任何 x86_64 系统上运行，并为许多用户提供广泛的功能集。 Plex、NextCloud 和 Asigra 等插件应用程序允许用户自定义系统功能。 | **TrueNAS Enterprise** 作为具有单控制器或双控制器的系统提供，以实现高可用性 (HA)。 它还可以从 iXsystems公司获得企业级支持。 结合 TrueNAS M 系列系统，以实现高达 15GB/s 的速度和 20 PB的存储空间，且可保证正常运行时间为 5 个 9（99.999%）。 | **TrueNAS SCALE** 是 TrueNAS 系列的最新成员，提供开源超融合基础架构，包括基于 Linux的Docker 容器和 虚拟机。 TrueNAS SCALE 包括系统集群功能，也提供容量高达数百 PB 的横向扩展存储的能力。 它目前正在开发中，将于 2021 年投入使用。至2021年11月4日，已发布RC.1.1版 |

TrueNAS 软件产品提供以下市场领先的功能，成为最有效的 NAS 解决方案：

#### **1、目前最强保护和性能的OpenZFS文件系统**

所有 TrueNAS 版本都利用企业级 OpenZFS 文件系统来满足包罗万象的数据管理需求。 为了在存储容量和数据安全之间提供完美的平衡，用户可以在各种 RAIDZ 或镜像配置中配置存储池。 在系统中添加新磁盘时，甚至可以扩展存储池，让您的存储环境随着应用需求而增长。

ZFS 使用诸如写时复制、数据校验和、数据擦除和多重数据等功能保护您的数据。 自动复制确保您的每一台服务器上的数据不会有1bit的差距；快速重建意味着如果磁盘出现故障，可以直接替换损坏的硬盘并快速重建数据存储池。 ZFS 还使用微型快照、克隆技术、在线文件压缩、自动精简配置和数据去重来最大化可用空间。

#### **2、以安全为中心，让您高枕无忧**

TrueNAS 支持多种安全解决方案，包括自加密驱动器 (SED) 和 基于ZFS的 本机数据集加密。 同时管理员还可以定制访问控制列表(ACL)为敏感数据提供另一层保护。 TrueNAS 还支持 FIPS 140-2 2 级驱动器，并在安装时默认采用包括文件传输加密和禁用 SSH等安全设置。TrueNAS支持管理本地用户和组，同时将 TrueNAS也可以集成LDAP统一身份认证、Active Directory活动目录（域服务）、Kerberos 和 NIS 。 对于大型企业，还可以使用集中式密钥管理（KMIP）。

#### **3、共享您的数据从未如此简单**

TrueNAS 支持所有著名的网络文件共享和远程备份选项，甚至可以使用来自 FreeBSD 或 Linux 的应用程序进行扩展。 支持的共享协议包括 NFS (v3,4)、SMB (v1,2,3,4)、AFP、FTP、WebDAV 和 rsync。 TrueNAS 还支持完整的基于硬件的共享（iSCSI、光纤通道），并已通过与 vSphere、Citrix 和 Veeam 一起使用的认证。 想要通过云存储提供商安全地导入或备份数据？ 没问题！ TrueNAS 甚至可以自动安排存储和同步您的 Amazon S3 数据。

#### **4、TrueNAS 提供丰富的功能**

每个 TrueNAS 版本都有大量的功能。 您可以阅读开源软件，以零成本在虚拟机中试用 TrueNAS，或者查看这张总结了关键特性的图表（TrueNAS 12.0 特性为蓝色）。

![](https://www.truenas.com/docs/images/CORE/Features.png)

TrueNAS SCALE 使用大部分相同的 TrueNAS CORE 源代码，但添加了一些由以下首字母缩略词定义的不同功能：

**S**cale-out ZFS----横向扩展 ZFS
**C**onverged----超融合
**A**ctive-active----主动式
**L**inux containers----Linux容器
**E**asy-to-manage----易于管理

#### 5、在任何硬件基础上运行 TrueNAS

TrueNAS 提供软件定义存储 (SDS) 并可以在任何 x86_64 服务器上运行，无论它有多旧或多现代。支持 Intel 和 AMD 处理器或任何一代处理器，无论是单核还是 64 核庞然大物。 TrueNAS 硬件指南提供了帮助您选择自己的硬件的建议。

TrueNAS 软件自 2009 年以来一直在 iXsystems 的赞助下开发，并使用了庞大的专业团队来开发、构建、QA、记录和支持开源软件和 TrueNAS 社区。由于 TrueNAS 软件是免费的，iXsystems 通过构建专业和企业级系统来发展其业务，这些系统具有与主要 NAS 供应商相似的可靠性，但总拥有成本 (TCO) 低得多。 iXsystems 的主张是 TrueNAS 通过开源经济显着节省存储成本。 TrueNAS 提供业界最强大的开放式存储。

TrueNAS 可以从[这个页面](https://www.truenas.com/download-tn-core/)下载。

## **加入我们的社区**

TrueNAS 包括开源软件和过去十几年开发的经验丰富的专家社区。 寻求建议并贡献您的经验供他人使用。 [社区](https://www.truenas.com/community/)可以为您节省很多时间，并成为新项目和应用程序的重要来源。 如果您需要更专业的支持，iXsystems 提供铜级/银级/金牌企业级支持，包括 24x365 服务和现场技术支持。

## **利用TrueCommand 管理您的 NAS 机群**

TrueCommand 是一个单一的可视化窗格管理应用程序，它通过在一个易于使用的界面中集中系统警报、报告和分析来消除多 TrueNAS 管理中的重复工作。 它通过 24x365 全球运营、基于角色的访问控制以及每个 TrueNAS 配置更改的完整日志来支持用户和团队。 TrueCommand 可在 docker容器、虚拟机或云服务上运行，对少于 50 个硬盘的用户免费，多于50个硬盘的用户可负担得起。 在[概述文章](https://www.truenas.com/docs/truecommand/)中阅读有关 TrueCommand 的更多信息。

![](https://www.truenas.com/docs/images/TrueCommand/Overview.png)
