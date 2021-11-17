# 虚拟局域网（VLAN）

虚拟 LAN (VLAN) 是在数据链路层（OSI 第 2 层）的计算机网络中划分和隔离的域。 可以在[此处](https://www.ieee802.org/1/pages/802.1Q-2014.html)找到有关 VLAN 的更多信息。 TrueNAS 使用 [vlan(4)](https://www.freebsd.org/cgi/man.cgi?vlan%284%29) 来管理 VLAN。

## 创建VLAN

要设置VLAN接口，请转至网络（**Network**） > 接口 （**Interface**）> 添加（**Add**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesAddVLAN.png)

将类型（**Type**）设置为 **VLAN** 并输入接口的名称。 该名称必须使用 vlanX 格式，其中 X 是表示非父接口的数字。 还建议在描述中添加有关此特定 VLAN 的任何注释或提醒。

启用 DHCP 或自动配置 IPv6 需要了解这个新接口将如何在您的特定网络环境中运行。 默认情况下，TrueNAS 只允许一个网络接口启用 DHCP。

必须配置剩余的 VLAN 设置以使接口正常运行：

- 物理接口（**Parent Interface**） : 选择 VLAN物理接口。 这通常是连接到已为 VLAN 配置的交换机端口的以太网卡。
- VLAN标签（**Vlan Tag**） : 为此接口输入数字标签。 这通常在交换网络中预先配置。
- 优先级（**Priority Code Point**）：定义 VLAN [服务等级](https://tools.ietf.org/html/rfc4761#section-4.2.7)。 可用的*802.1p* 服务等级范围从尽力而为（**Best effort**）（默认） 到网络控制（**Network control**）（最高）。

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
