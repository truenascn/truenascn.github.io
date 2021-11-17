# 全局设置

网络（**Network**） > 全局设置（**Global Configuration**）具有不依赖于任何接口的所有常规 TrueNAS 网络设置。

**注意：更改 Web 界面使用的网络界面可能会导致与 TrueNAS 的连接丢失！ 修复任何错误配置的网络设置可能需要命令行知识或对 TrueNAS 系统的物理访问。**

## 全局配置设置

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkGlobalConfiguration.png)

选项分为几个类别。

这些接口、DNS 和网关选项也在[控制台设置菜单](https://www.truenas.com/docs/core/gettingstarted/consolesetupmenu/)中配置。 在对网络连接问题进行故障排除时，请务必检查这两个位置。

### 主机名和域名

主机名和域名有默认值，但可以更改以满足本地网络的要求。 主机名和域显示在仪表板（**Dashboard**） > 系统信息（**System Information**）卡片中。 某些选项仅在适当的硬件存在时才显示。

| 设置选项                                          | 值            | 描述                                                         |
| ------------------------------------------------- | ------------- | ------------------------------------------------------------ |
| 主机名（**Hostname**）                            | 字符串/string | 第一个 TrueNAS 控制器的主机名。 允许使用大小写字母数字、`.` 和 `-` 字符。 |
| 第二主机名（**Hostname (TrueNAS Controller 2)**） | 字符串/string | 第二个 TrueNAS 控制器的主机名（仅适用于高可用）。 允许使用大小写字母数字、`.` 和 `-` 字符。 |
| 虚拟主机名（**Hostname (Virtual)**）              | 字符串/string | 虚拟主机名。 使用虚拟主机时，这也用作 Kerberos 主体名称。 输入完全限定的主机名和域名。 允许使用大小写字母数字、`.` 和 `-` 字符。 |
| 域名（**Domain**）                                | 字符串/string | 系统域名。                                                   |
| 附加域名（**Additional Domains**）                | 字符串/string | 其他搜索域，按 Enter 分隔条目。 添加搜索域可能会导致 DNS 查找速度变慢 |

### 服务通告

| 设置选项                            | 值              | 描述                                                         |
| ----------------------------------- | --------------- | ------------------------------------------------------------ |
| NetBIOS名称服务器（**NetBIOS-NS**） | 复选框/checkbox | 旧版 NetBIOS 名称服务器。 通告 SMB 服务 NetBIOS 名称。 可能需要旧版 SMB1 客户端来发现服务器。勾选后，服务器会出现在网上邻居中。 |
| 多播DNS（**mDNS**）                 | 复选框/checkbox | 多播 DNS。 使用系统 *Hostname* 来通告已启用和正在运行的服务。 例如，这控制服务器是否出现在 MacOS 客户端上的 **Network** 中。 |
| Windows发现（**WS-Discovery**）     | 复选框/checkbox | 使用 SMB 服务的*NetBIOS 名称 * 将服务器通告给 WS-Discovery 客户端。 这会导致计算机出现在现代 Windows 操作系统的**网上邻居**中。 |

DNS服务器

| 设置选项                        | 值                | 描述              |
| ------------------------------- | ----------------- | ----------------- |
| DNS服务器 1（**Nameserver 1**） | IP地址/IP address | 首选 DNS 服务器。 |
| DNS服务器 2（**Nameserver 2**） | IP地址/IP address | 辅助 DNS 服务器。 |
| DNS服务器 3（**Nameserver 3**） | IP地址/IP address | 第三 DNS 服务器。 |

默认网关

| 设置选项                                 | 值                | 描述                                                  |
| ---------------------------------------- | ----------------- | ----------------------------------------------------- |
| IPV4默认网关（**IPv4 Default Gateway**） | IP地址/IP address | 通常不设置。 如果设置，则代替 DHCP 提供的默认网关使用 |
| IPv6默认网关（**IPv6 Default Gateway**） | IP地址/IP address | 通常不设置。                                          |

其它设置

| 设置选项                                       | 值              | 描述                                                         |
| ---------------------------------------------- | --------------- | ------------------------------------------------------------ |
| HTTP代理服务器（**HTTP Proxy**）               | 字符串/string   | 以 *`http://my.proxy.server:3128`* 或 *`http://user:password@my.proxy.server:3128`* 格式输入网络的代理信息。 |
| 启用网络等待功能（**Enable Netwait Feature**） | 复选框/checkbox | 设置此项可防止网络服务启动，直到接口可以 ping通 网络等待 IP 列表（**Netwait IP 列表**）中列出的地址。 |
| 网络等待 IP 列表（**Netwait IP List**）        | 字符串/string   | 仅在设置了启用网络等待功能（**Enable Netwait Feature**）时出现。 输入要 ping 的 IP 地址列表。 按 Enter 分隔条目。 尝试每个地址，直到成功或列表用完为止。 留空以使用默认网关。 |
| 主机名数据库（**Host Name Database**）         | 字符串/string   | 用于每行添加一个条目，该条目将附加到 /etc/hosts。 按 Enter 分隔条目。 使用 *`IP_address space hostname`* 格式，如果用空格分隔，可以使用多个主机名。 即使 DNS 不可用，此处定义的主机仍可通过名称访问。 有关其他信息，请参阅 [hosts](https://www.freebsd.org/cgi/man.cgi?query=hosts)。 |







