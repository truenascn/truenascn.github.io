# 基于光纤通道的iSCSI

光纤通道是一种高速数据传输协议，提供原始块数据的有序、无损传输。 光纤通道主要用于将计算机数据存储连接到商业数据中心存储区域网络中的服务器。 光纤通道协议在各种存储工作负载中快速、经济且可靠。

可以使用光纤通道的TrueNAS 产品：

- [TrueNAS R-Series](https://www.truenas.com/r-series/)(4x16 Gbps)
- [TrueNAS X-10](https://www.truenas.com/x-series/)(2x8 Gbps)
- [TrueNAS X-20](https://www.truenas.com/x-series/)(2x8 Gbps)
- [TrueNAS M-40](https://www.truenas.com/m-series/)(4x16 Gbps)
- [TrueNAS M-50](https://www.truenas.com/m-series/)(4x16 Gbps or 2x32 Gbps)
- [TrueNAS M-60](https://www.truenas.com/m-series/)(4x32 Gbps)

这是 TrueNAS Enterprise 的一项功能。 获得光纤通道许可的 TrueNAS 系统将光纤通道端口添加到共享（**Sharing**） > 块共享 (iSCSI)（**Block Shares (iSCSI)**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIFibreChannelPorts.png)

## 光纤通道 ISCSI 共享示例

发起者（**Initiators**）和授权访问（**Authorized Access**）页面仅适用于 iSCSI，在配置光纤通道时可以忽略。

转到存储（**Storage**） > 存储池（**Pools**）。 找到现有池，单击右侧的三个小点（选项），然后单击添加 zvol（**Add zvol**） 以创建新的 zvol。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsZvolFibreEnterprise.png)

在共享（**Sharing**） > 块共享 (iSCSI)（**Block Shares (iSCSI)**）中配置这些选项卡：

### 门户（Portals）

如果侦听接口为 `0.0.0.0:3260` 的门户不存在，请单击添加（**Add**）并添加此门户。

### 发起者组（Initiators Groups）

| 设置                                     | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 连接的发起者（**Connected Initiators**） | 当前连接到系统的启动器。 以 IQN 格式显示，带有 IP 地址。 设置启动器并单击 ->（箭头）将启动器添加到“允许的启动器”或“授权的网络”列表中。 单击刷新更新连接的启动器列表。 |
| 允许的发起者（**Allowed Initiators**）   | 发起者允许访问该系统。 输入 [iSCSI 限定名称 (IQN)](https://tools.ietf.org/html/rfc3720#section-3.2.6) 并单击 + 将其添加到列表中。 示例：iqn.1994-09.org.freebsd:freenas.local |
| 授权网络（**Authorized Networks**）      | 允许的网络地址使用此发起程序。 每个地址都可以包含一个可选的 CIDR 网络掩码。 单击 + 将网络地址添加到列表中。 示例：192.168.2.0/24。 |
| 描述（**Description**）                  | 关于启动器的任何注释。                                       |

### 授权访问（Authorized Access）

| 设置                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 用户组ID（**Group ID**）    | 允许使用不同的身份验证配置文件配置不同的组。 示例：组 ID 为 1 的所有用户都将继承与组 1 关联的身份验证配置文件。 |
| 用户（**User**）            | 为与远程系统上的用户进行 CHAP 身份验证而创建的用户帐户。 许多启动器使用启动器名称作为用户名。 |
| 密码（**Secret**）          | 用户密码。 长度必须至少为 12 个字符且不超过 16 个字符。      |
| 对等用户（**Peer User**）   | 仅在配置双向 CHAP 时输入。 通常与用户相同的值。              |
| 对等密码（**Peer Secret**） | 互密密码。 设置对等用户时需要。 必须与秘密不同。             |

### 目标（Targets）

添加（**Add**）一个新的目标

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSITargetsAddFibre.png)

为目标名称（**Target Name**）、目标别名（**Target Alias**）、目标模式（**Target Mode**）和门户组（**Portal Group**）输入或选择特定于您的用例的值。 启动器组 ID（**Initiator Group ID**） 选择哪个现有启动器组可以访问目标。 身份验证方法（**Authentication Method**）的选项为无、自动、CHAP 或相互 CHAP。 授权组数量（**Authentication Group Number**） 可以设置为 none 或整数。 此值表示现有授权访问的数量。

转到目标（**Targets**）并单击添加（**ADD**）后，会出现一个额外的目标模式（**arget Mode**）选项。 这个新选项用于选择要创建的目标是 iSCSI、光纤通道还是两者兼而有之。

目标（**Targets**）[报告](https://www.truenas.com/docs/core/administration/reporting/)选项卡提供光纤通道端口带宽图。

### 范围（Extents）

添加（**Add**）一个新的范围

![](https://www.truenas.com/docs/images/CORE/12.0/ISCSIExtentsAddFibre.png)

| 设置                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 名称（**Name**）                                             | 范围的名称。 如果 *Extent* 大小不为 0，则它不能是池或数据集中的现有文件。 |
| 描述（**Description**）                                      | 关于这个范围的注释。                                         |
| 启用（**Enabled**）                                          | 设置为启用这个 iSCSI 范围。                                  |
| 范围类型（**Extent Type**）                                  | 设备（**Device**）提供对ZFS卷（**zvol**）、zvol 快照或物理设备的虚拟存储访问。 文件（**File**） 提供对单个文件的虚拟存储访问。 |
| 设备（**Device**）                                           | 仅在选择设备（**Device**） 时出现。 选择未格式化的磁盘、控制器或 zvol 快照。 |
| 逻辑块大小（**Logical Block Size**）                         | 除非发起者需要不同的块大小，否则保留默认值 512。             |
| 禁止物理块大小报告（**Disable Physical Block Size Reporting**） | 如果发起者不支持超过 4K (MS SQL) 的物理块大小值，则勾选此项。 |
| 启用TPC（**Enable TPC**）                                    | 设置为允许发起者绕过正常访问控制并访问任何可扫描目标。 这允许 [xcopy](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc771254(v=ws.11) )。 |
| Xen 发起者兼容模式（**Xen initiator compat mode**）          | 使用 Xen 作为 iSCSI 启动器时设置。                           |
| **LUN RPM**                                                  | 当使用 Windows 作为发起者时，**不要**更改此设置。 只有在需要使用特定 RPM 的系统数量才能准确报告统计数据的大型环境中，才需要更改。 |
| 只读（**Read-only**）                                        | 设置为阻止启动器初始化此 LUN。                               |

### 相关目标（Associated Targets）

添加（**Add**）新的相关目标。

![](https://www.truenas.com/docs/images/CORE/12.0/ISCSIAssocTargetAddFibre.png)

为目标和范围选择值。 LUN ID 是一个介于 0 和 1023 之间的值。一些发起者期望值低于 256。将此字段留空以自动分配下一个可用 ID。

### 光纤通道端口（Fibre Channel Ports）

单击 > 展开选项，选择测试数据下显示的选项，然后保存。

未开启服务时，iSCSI 共享不起作用。 要打开 iSCSI 服务，请转至服务（**Services**）并切换启动 **iSCSI**。

## NPIV（N_Port ID 虚拟化）

NPIV 允许管理员使用交换机分区来配置每个虚拟端口，就好像它是一个物理端口一样，以提供访问控制。 这在混合了 Windows 系统和虚拟机的环境中很重要，以防止自动或意外重新格式化包含无法识别的文件系统的目标。 它还可以用于隔离数据； 例如，防止工程部门访问人力资源部门的数据。 有关如何配置虚拟端口分区的详细信息，请参阅交换机文档。

要在 TrueNAS 系统上创建虚拟端口，请转至系统（**System**） > 可调参数（**Tunables**）并单击添加（**ADD**）。 输入这些选项：

- 变量名（**Variable**） : `input hint.isp.X.vports`, 将 *X* 替换为物理接口的编号。
- 值（**Value**） :输入要创建的虚拟端口数。 不能超过 *125* 个 SCSI 目标端口，包括所有物理光纤通道端口、所有虚拟端口以及 iSCSI 门户和目标的所有配置组合。
- 类型（**Type**） : 确保选择了 *loader*。

![](https://www.truenas.com/docs/images/CORE/11.3/SystemTunablesFibre.png)

在所示示例中，两个物理接口各分配了 4 个虚拟端口。 需要两个可调参数，每个物理接口一个。 创建可调参数后，配置的虚拟端口数会出现在共享（**Sharing**） > 块共享 (iSCSI)（**Block Shares (iSCSI)**）> 光纤通道端口（**Fibre Channel Ports**）页面中，以便它们可以与目标相关联。 它们也被通告给交换机，因此可以在交换机上配置分区。

虚拟端口与目标关联后，它会添加到[报告](https://www.truenas.com/docs/core/administration/reporting/)的目标（**Target**）选项卡中，可以在其中查看其带宽使用情况。
