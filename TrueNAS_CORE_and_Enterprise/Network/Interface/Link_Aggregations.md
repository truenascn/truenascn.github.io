# 链路聚合

[链路聚合 (LAGG)](https://tools.ietf.org/html/rfc7424) 是一种将多个并行或串联网络连接组合（聚合）的通用方法，可为关键网络情况提供额外的带宽或冗余。 TrueNAS 使用 [lagg(4)](https://www.freebsd.org/cgi/man.cgi?lagg%284%29) 来管理 LAGG。

## 设置链路聚合

要设置 LAGG 接口，请转至网络（**Network**） > 接口 （**Interface**）> 添加（**Add**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesAddLAGG.png)

将类型（**ype**）设置为链路聚合（**Link Aggregation**）。

输入接口的名称。 该名称必须使用格式 laggX，其中 X 是表示非父接口的数字。 还建议在描述中添加有关此特定 LAGG 的任何注释或提醒。

在 LAGG 设置下，设置 Lagg 协议（**Lagg Protocol**）以配置接口端口以满足您的网络需求。

## LGAA协议

### LACP

最常用的 LAGG 协议，是 IEEE 规范 802.3ad 的一部分。 在 LACP 模式下，与网络交换机进行协商，形成一组同时处于活动状态的端口。 网络交换机必须支持 LACP 才能使该选项起作用。

### 故障转移

故障转移（**Failover**）导致流量通过组的主要接口发送。 如果主接口出现故障，流量将转移到 LAGG 中的下一个可用接口。

### 负载均衡

负载均衡（**Load Balance**）在 LAGG 组的任何端口上接受入站流量，然后在 LAGG 组中的活动端口上平衡出站流量。 它是一种静态设置，既不监视链路状态也不与交换机协商。

### 轮询调度

轮询调度（**RoundRobin**） LAGG 组任何端口上的入站流量，并使用循环调度算法发送出站流量。 流量依次使用每个 LAGG 接口依次发出。

### 无

无（**None**）该模式禁用 LAGG 接口上的流量，但不禁用 LAGG 接口。

现在定义滞后接口并查看剩余的接口选项。

## 其它设置

每种网络接口都有通用设置：

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesAddOtherSettings.jpg)

不鼓励禁用硬件卸载（**Hardware Offloading**），因为它会降低网络性能。 但是，当界面管理 [Jails](https://www.truenas.com/docs/core/applications/jails/create/)、[插件](https://www.truenas.com/docs/core/applications/plugins/manageplugins/)或[虚拟机](https://www.truenas.com/docs/core/applications/virtualmachines/basic/)时，可能需要禁用此选项。

最大传输单元 (MTU) 是可以通信的最大协议数据单元。 最大可用的 MTU 大小将根据您的可用网络接口和其他物理硬件而变化。 1500 和 9000 是标准以太网 MTU 大小，建议使用默认值 1500。MTU 值的允许范围是 1492-9216。 将此字段留空会将默认值设置为 1500。

如果需要额外调整，您可以在选项中输入额外的 [ifconfig](https://www.freebsd.org/cgi/man.cgi?query=ifconfig) 设置

## IP地址

还可以定义接口的其他别名：

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesAddIpAddresses.jpg)

可以定义 IPv4 或 IPv6 地址和 1-32 的子网。 单击添加（**ADD**）将提供另一个用于定义 IP 地址的字段。