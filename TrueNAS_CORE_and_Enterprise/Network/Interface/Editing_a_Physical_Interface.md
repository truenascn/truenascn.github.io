# 编辑物理接口

## 接口编辑

配置控制 TrueNAS® 网络界面的网络界面时要小心，否则您可能会失去网络连接。
网络（**Network**） > 接口 （**Interface**）列出连接到 TrueNAS® 系统的所有物理[网络接口控制器 (NIC)](https://www.truenas.com/docs/core/network/interfaces/)。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceOverviewPage.png)

要编辑接口，请单击它旁边的 > 以展开视图并提供有关所选接口的一般描述，然后单击“编辑”（**EDIT**）。

如果您是 TrueNAS Enterprise 客户，请记住，如果启用了高可用性 (HA)，您将无法编辑网络接口。
转至系统 （**System**）> 故障转移（**Failover**）并选中禁用故障转移（**Disable Failover**）复选框，然后单击保存（**SAVE**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceDescriptionView.png)

接口的编辑选项取决于您正在修改的接口类型。

## 选项

### 接口设置

| 设置选项                               | 值              | 描述                                                         |
| -------------------------------------- | --------------- | ------------------------------------------------------------ |
| 描述（**Description**）                | 字符串/string   | 关于此网络接口的注释或说明文字。                             |
| 动态主机分配协议（**DHCP**）           | 复选框/checkbox | 启用 DHCP 以自动为该接口分配 IPv4 地址。 保留未设置以创建静态 IPv4 或 IPv6 配置。 只能在一个接口上配置DHCP。 |
| IPv6自动配置（**Autoconfigure IPv6**） | 复选框/checkbox | 使用 [rtsol](https://www.freebsd.org/cgi/man.cgi?query=rtsol) 自动配置 IPv6 地址。 这种方式只能配置一个接口。 |

### 其它设置

| 设置选项                                        | 值              | 描述                                                         |
| ----------------------------------------------- | --------------- | ------------------------------------------------------------ |
| 禁用硬件卸载（**Disable Hardware Offloading**） | 复选框/checkbox | 关闭网络流量处理的硬件卸载。 警告：禁用硬件卸载会降低网络性能，仅在需要管理部分[Jails](https://www.truenas.com/docs/core/applications/jails/)、[插件](https://www.truenas.com/docs/core/applications/plugins/)或[虚拟机 (VM)](https://www.truenas.com/docs/core/applications/virtualmachines/) 时才推荐使用。 |
| 最大传输单元（**MTU**）                         | 字符串/string   | 最大传输单元，可以通信的最大协议数据单元。 最大可用的 MTU 大小因网络接口和设备而异。 1500 和 9000 是标准以太网 MTU 大小。 留空会将字段恢复为默认值 1500。 |
| 选项（**Options**）                             | 字符串/string   | 来自 [ifconfig](https://www.freebsd.org/cgi/man.cgi?query=ifconfig) 的附加参数。 用空格分隔多个参数。 例如：mtu 9000 增加支持巨型帧的接口的 MTU。 |

### IP地址

| 设置选项                 | 值                                        | 描述                                                         |
| ------------------------ | ----------------------------------------- | ------------------------------------------------------------ |
| IP地址（**IP Address**） | 整数和下拉菜单/integer and drop-down menu | 静态 IPv4 或 IPv6 地址和子网掩码。 示例：10.0.0.3 和 /24。 单击添加（**ADD**） 添加另一个 IP 地址。 单击删除（**DELETE**） 将删除该 IP 地址。 |

## 保存更改

完成编辑后，单击“保存”（**SAVE**）。 您可以选择测试更改（**TEST CHANGES**）或还原更改（**REVERT CHANGES**）。 测试更改的默认时间为 60 秒，但您可以将其更改为所需的设置。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceTestChanges.png)

单击测试更改（**TEST CHANGES**）后，确认您的选择并再次单击测试更改（**TEST CHANGES**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceTestChangesNotice.png)

用户可以保存更改（**SAVE CHANGES**）或还原更改（**REVERT CHANGES**）。 用户将有他们指定的时间来做出选择。 如果您选择 **SAVE CHANGES**，则会出现一个对话框，要求您取消（**CANCEL**）或保存（**SAVE**）网络接口更改。 单击保存（**SAVE**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceEditSaveChanges.png)

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceSaveChangesOption.png)

系统将显示一个对话框，显示网络接口更改已永久生效。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaceDialogBox.png)
