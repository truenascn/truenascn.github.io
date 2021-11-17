# 网络概览

建议在设置数据共享之前设置系统的网络连接。 这允许在尝试存储或共享关键数据之前将 TrueNAS 集成到您的特定网络环境中。

## 概览页面

网络摘要简要概述了当前的网络设置。 提供了有关当前活动接口、默认路由和名称服务器的信息。 这些内容在此处不可编辑。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkSummary.png)

- 接口（**Interfaces**）显示任何已配置的物理接口、[网桥](https://www.truenas.com/docs/core/network/interfaces/bridgecreate/)、[LAGG](https://www.truenas.com/docs/core/network/interfaces/laggcreate/) 和 [vlan](https://www.truenas.com/docs/core/network/interfaces/vlancreate/)。 即使未配置，也会列出所有检测到的物理接口。 当为接口配置了[静态 IP](https://www.truenas.com/docs/core/network/interfaces/settingstaticip/) 时，会显示 IPv4 或 IPv6 地址。
- 默认路由（**Default Routes**）列出了所有保存的 TrueNAS 默认路由。 转至网络（**Network**） > 全局设置（**Global Configuration**）以配置默认路由。
- 名称服务器（**Nameservers**） 列出了 TrueNAS 使用的任何配置的 DNS 服务器。 要更改此列表，请转至网络（**Network**） > 全局设置（**Global Configuration**）。 TrueNAS 主机名和域、默认网关和其他选项可以在网络（**Network**） > 全局设置（**Global Configuration**）中修改。

## 附加网络配置页面

可以在网络（**Network**） > 静态路由（**[Static Routes](https://www.truenas.com/docs/core/network/staticroutes/)**）中定义任何静态路由。

带外管理通过网络（**Network**） > [**IPMI**](https://www.truenas.com/docs/core/network/ipmi/) 进行管理。 此选项仅在 TrueNAS 检测到适当的物理硬件时可见。

