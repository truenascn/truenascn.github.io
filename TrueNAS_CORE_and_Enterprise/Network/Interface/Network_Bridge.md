# 网桥

[网桥](https://tools.ietf.org/html/rfc6325)一般是指将多个网络连接组合成单个网络的方法。 TrueNAS 使用 [bridge(4)](https://www.freebsd.org/cgi/man.cgi?bridge%284%29) 来管理网桥。

## 创建网桥

要设置桥接接口，请转至网络（**Network**） > 接口 （**Interface**）> 添加（**Add**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesAddBridge.png)

将类型（**Type**）设置为桥接（**Bridge**）并输入接口名称。 该名称必须使用格式 bridgeX，其中 X 是代表非父接口的数字。 还建议在描述中添加有关此网桥的任何注释或提醒。

在桥接设置（**Bridge Settings**）下，选择哪些接口将成为桥接成员，然后配置其余接口选项以满足您的网络需求。

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