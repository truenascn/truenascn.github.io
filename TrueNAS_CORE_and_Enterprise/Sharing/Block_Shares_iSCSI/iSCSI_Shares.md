# iSCSI 共享

首先，请确保您已创建一个 [zvol](https://www.truenas.com/docs/core/storage/pools/zvols/) 或至少包含一个要共享的文件的[数据集](https://www.truenas.com/docs/core/storage/pools/datasets/)。

转至共享（**Sharing**） > 块共享 (iSCSI)（**Block Shares (iSCSI)**）。 您可以手动设置共享，也可以使用向导来指导您完成创建。

## 向导设置（Wizard Setup）

### 1、块设备（Block Device）

首先，输入 iSCSI 共享的名称。 它只能包含小写字母数字字符和点 (.)、破折号 (-) 或冒号 (:)。 我们建议保持名称简短或最多 63 个字符。 接下来，选择范围类型。

- 如果范围类型（**Extent Type**） 是设备（**Device**），请从设备（**Device**）菜单中选择要共享的 ZFS卷（**Zvol**）。
- 如果范围类型（**Extent Type**）是 文件（**File**），选择 范围（**Extent**） 的路径并指明文件大小。

选择将使用共享的平台类型。 例如，如果使用来自更新的 Linux 操作系统的共享，请选择现代操作系统（**Modern OS**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIWizardDevice.png)

### 2、门户（Portal）

现在，您将创建一个新门户（**Portal**）或从下拉列表中选择一个现有门户（**Portal**）。

如果您创建新门户（**Portal**），则需要选择发现身份验证方法（**Discovery Authentication Method**）。

如果您将发现身份验证方法（**Discovery Authentication Method**）设置为 **CHAP** 或 **MUTUAL CHAP**，您还需要选择一个发现认证组**Discovery Authentication Group**）。 如果不存在组，请从下拉列表中单击新建并输入组 ID（**Group ID**）、用户（**User**）和机密（**Secret**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIWizardPortal.png)

当发现身份验证方法（**Discovery Authentication Method**）为无（**NONE**） 时，发现认证组**Discovery Authentication Group**）可以留空。

从 IP 地址下拉列表中选择 0.0.0.0 或 ::，然后单击下一步。

**Q**：这些选项是什么？

**A**：0.0.0.0 监听所有 IPv4 地址，:: 监听所有 IPv6 地址。

### 3、发起者（Initiator）

决定哪些启动器或网络可以使用 iSCSI 共享。 将列表留空以允许所有启动器或网络，或向列表添加条目以限制对这些系统的访问。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIWizardInitiator.png)

### 4、确认（Confirm）

确认设置正确，然后单击提交（**SUBMIT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIWizardSummary.png)

## 手动设置

### 目标全局配置（Target Global Configuration）

目标全局配置（**Target Global Configuration**）选项卡允许用户配置将应用于所有 iSCSI 共享的设置。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualTargetGlobalConfig.png)

| 设置                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 基本名称（**Base Name**）                                    | 允许使用小写字母数字字符加上点 (.)、破折号 (-) 和冒号 (:)。 请参阅 [RFC3721](https://tools.ietf.org/html/rfc3721.html) 的使用 *iqn.format* 部分构建 iSCSI 名称。 |
| ISNS 服务器（**ISNS Servers**）                              | 要向系统的 iSCSI 目标和门户注册的 ISNS 服务器的主机名或 IP 地址。 按 Enter 分隔条目。 |
| 池可用空间阈值 (%)（**Pool Available Space Threshold (%)**） | 当存储池的剩余空间到达此百分比时生成警报。 这通常在使用 zvol 时在池级别配置，或在基于文件和设备的范围的范围级别配置。 |

### 门户（Portals）

门户（**Portal**）选项卡允许用户创建新门户（**Portal**）或编辑列表中的现有门户（**Portal**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualPortals.png)

要添加新门户（**Portal**），请单击添加（**ADD**）并输入基本和 IP 地址信息。

要编辑现有门户（**Portal**），请单击门户（**Portal**）旁边的三个点（选项） 并选择编辑（**Edit**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualPortalsForm.png)

#### 基础信息（Basic info）

| 设置                    | 描述                                  |
| ----------------------- | ------------------------------------- |
| 描述（**Description**） | 可选说明。 门户会自动分配一个数字组。 |

#### 身份验证方法和组（Authentication Method and Group）

| 设置                                                    | 描述                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| 发现身份验证方法（**Discovery Authentication Method**） | iSCSI 支持目标用于发现有效设备的多种身份验证方法。 **None** 允许匿名发现，而 **CHAP** 和 **Mutual CHAP** 需要身份验证。 |
| 发现认证组**Discovery Authentication Group**）          | 在授权访问中创建的组 ID。 当 发现身份验证方法（**Discovery Authentication Method**） 为 **CHAP** 或 **Mutual CHAP** 时需要。 |

#### IP地址（IP Address）

| 设置                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| IP地址（**IP Address**） | 选择门户要侦听的 IP 地址。 单击添加以添加具有不同网络端口的 IP 地址。 *0.0.0.0* 监听所有 IPv4 地址，*::* 监听所有 IPv6 地址。 |
| 端口（**Port**）         | 用于访问 iSCSI 目标的 TCP 端口。 默认值为 *3260*。           |
| 添加（**ADD**）          | 添加另一个 IP 地址行。                                       |

### 发起者组（Initiators Groups）

发起者组（**Initiators Groups**） 选项卡允许用户创建新的授权访问客户端组或编辑列表中的现有组。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualInitiators.png)

要添加新的发起者组（**Initiators Groups**），请单击“添加”（**ADD**）并选中“允许所有发起者”（**Allow All Initiators**）或配置您自己允许的发起者和授权网络。

要编辑现有发起者组（**Initiators Groups**），请单击发起者组旁边的三个小点（选项） 并选择编辑（**Edit**）。

| 设置                                         | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| 允许所有发起者（**Allow All Initiators**）   | 选中时允许所有发起者。                                       |
| 允许的发起者（**Allowed Initiators (IQN)**） | 允许访问该系统的发起者。 输入 [iSCSI 限定名称 (IQN)](https://tools.ietf.org/html/rfc3720#section-3.2.6) 并单击 *+* 将其添加到列表中。 示例：*iqn.1994-09.org.freebsd:freenas.local*。 |
| 授权网络（**Authorized Networks**）          | 允许使用此发起程序的网络地址。 每个地址都可以包含一个可选的 [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) 网络掩码。 单击 + 将网络地址添加到列表中。 示例：*192.168.2.0/24*。 |
| 描述（**Description**）                      | 关于启动器的任何注释。                                       |

### 授权访问（Authorized Access）

授权访问（**Authorized Access**）选项卡允许用户创建新的授权访问网络或编辑列表中的现有网络。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualAuthorizedAccess.png)

#### 组（Group）

| 设置                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| 用户组ID（**Group ID**） | 允许使用不同的身份验证配置文件配置不同的组。 示例：组 ID 为 1 的所有用户都将继承与组 1 关联的身份验证配置文件。 |

#### 用户（User）

| 设置                             | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 用户（**User**）                 | 为与远程系统上的用户进行 CHAP 身份验证而创建的用户帐户。 许多启动器使用启动器名称作为用户名。 |
| 密码（**Secret**）               | 用户密码。 长度必须至少为 12 个字符且不超过 16 个字符。      |
| 确认密码（**Secret (Confirm)**） | 确认用户密码。                                               |

#### 对等用户（Peer User）

| 设置                                      | 描述                                             |
| ----------------------------------------- | ------------------------------------------------ |
| 对等用户（**Peer User**）                 | 仅在配置双向 CHAP 时输入。 通常与用户相同的值。  |
| 对等密码（**Peer Secret**）               | 互密密码。 设置对等用户时需要。 必须与秘密不同。 |
| 确认对等密码（**Peer Secret (Confirm)**） | 确认互密密码。                                   |

### 目标（Targets）

目标（Targets）选项卡允许用户创建新的 TrueNAS 存储资源或编辑列表中的现有资源。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualTargets.png)

要添加新目标，请单击添加（**ADD**）并输入基本和 iSCSI 组信息。

要编辑现有目标，请单击它旁边的 三个小点（选项） 并选择编辑（**Edit**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualTargetsForm.png)

#### 基本信息（Basic Info）

| 设置                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| 目标名称（**Target Name**）  | 如果目标名称不以 iqn 开头，则会自动添加基本名称。 允许使用小写字母数字字符加上点 (.)、破折号 (-) 和冒号 (:)。 请参阅 [RFC3721](https://tools.ietf.org/html/rfc3721.html) 的使用 iqn.format 部分构建 iSCSI 名称。 |
| 目标别名（**Target Alias**） | 可选的用户友好名称。                                         |

#### iSCSI组（iSCSI Group）

| 设置                                          | 描述                                          |
| --------------------------------------------- | --------------------------------------------- |
| 门户组（**Portal Group ID**）                 | 留空或选择要使用的现有门户。                  |
| 发起者组ID（**Initiator Group ID**）          | 选择可以访问目标的现有发起者组。              |
| 授权方法（**Authentication Method**）         | 选项包括*无*、*自动*、*CHAP* 或*相互 CHAP*。  |
| 授权组数量（**Authentication Group Number**） | 选择*无*或整数。 此值表示现有授权访问的数量。 |

### 范围（Extents）

范围（Extents）选项卡允许用户创建新的共享存储单元或编辑列表中的现有存储单元。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualExtents.png)

要添加新范围，请单击添加（**ADD**）并输入基本、类型和兼容性信息。

要编辑现有范围，请单击它旁边的三个小点（选项） 并选择编辑（**Edit**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualExtentsForm.png)

#### 基本信息（Basic Info）

| 设置                    | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 名称（**Name**）        | 范围的名称。 如果 *Extent* 大小不为 0，则它不能是池或数据集中的现有文件。 |
| 描述（**Description**） | 关于这个范围的注释。                                         |
| 启用（**Enabled**）     | 设置为启用这个 iSCSI 范围。                                  |

#### 类型（Type）

| 设置                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 范围类型（**Extent Type**）                                  | 设备（**Device**）提供对ZFS卷（**zvol**）、zvol 快照或物理设备的虚拟存储访问。 文件（**File**） 提供对单个文件的虚拟存储访问。 |
| 设备（**Device**）                                           | 仅在选择设备（**Device**） 时出现。 选择未格式化的磁盘、控制器或 zvol 快照。 |
| 范围的路径（**Path to the Extent**）                         | 仅在选择 *File* 时出现。 浏览到现有文件。 通过浏览到数据集并将 /{filename.ext} 附加到路径来创建一个新文件。 用户不能在 jail 根目录中创建扩展区。 |
| 文件大小（**Filesize**）                                     | 仅在选择 *File* 时出现。 输入 0 使用实际文件大小并要求该文件已存在。 否则，指定新文件的文件大小。 |
| 逻辑块大小（**Logical Block Size**）                         | 除非发起者需要不同的块大小，否则保留默认值 512。             |
| 禁用物理块大小报告（**Disable Physical Block Size Reporting**） | 如果发起者不支持超过 4K (MS SQL) 的物理块大小值，则勾选此项。 |

#### 兼容性（Compatibility）

| 设置                                                | 描述                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| 启用 TPC（**Enable TPC**）                          | 设置为允许发起者绕过正常访问控制并访问任何可扫描目标。 这允许 [xcopy](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc771254(v=ws.11) )。 |
| Xen 发起者兼容模式（**Xen initiator compat mode**） | 使用 Xen 作为 iSCSI 启动器时设置。                           |
| **LUN RPM**                                         | 当使用 Windows 作为发起者时，**不要**更改此设置。 只有在需要使用特定 RPM 的系统数量才能准确报告统计数据的大型环境中，才需要更改。 |
| 只读（**Read-only**）                               | 设置为阻止启动器初始化此 LUN。                               |

### 相关目标（Associated Targets）

相关目标（Associated Targets）选项卡允许用户创建新的关联 TrueNAS 存储资源或编辑列表中的现有资源。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualAssociatedTargets.png)

要添加新的关联目标，请单击添加（**ADD**）并填写信息。

要编辑现有的关联目标，请单击它旁边的三个小点（选项） 并选择编辑（**Edit**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingISCSIManualAssociatedTargetsForm.png)

| 设置               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| 目标（**Target**） | 选择一个现有的目标。                                         |
| **LUN ID**         | 选择值或输入 0 到 1023 之间的值。一些发起者期望值低于 256。将此字段留空可以自动分配下一个可用 ID。 |
| 范围（**Extent**） | 选择现有范围。                                               |

## 使用 iSCSI 共享

连接和使用 iSCSI 共享可能因操作系统而异：

### Linux

#### iSCSI 实用程序和服务

首先，打开命令行并确保安装了 open-iscsi 实用程序。 要在 Ubuntu/Debian 发行版上安装该实用程序，请输入 `sudo apt update && sudo apt install open-iscsi`。 安装完成后，确保 iscsid 服务正在运行：`sudo service iscsid start`。 启动 iscsid 服务后，运行带有发现参数的 `iscsiadm` 命令并获取连接到共享所需的信息。

![](https://www.truenas.com/docs/images/CORE/LinuxISCSIAppInstall.png)

#### 发现并登录 iSCSI 共享

运行命令 `sudo iscsiadm \--mode discovery \--type sendtargets \--portal {IPADDRESS}`。 输出提供了 TrueNAS 配置的基本名称和目标名称。

![](https://www.truenas.com/docs/images/CORE/LinuxISCSIDiscoveryList.png)

或者，输入 `sudo iscsiadm -m discovery -t st -p {IPADDRESS}` 以获得相同的输出。 请注意输出中给出的基本名称和目标名称，因为您需要它们来登录 iSCSI 共享。

当 发现身份验证方法（**Discovery Authentication Method**）为 CHAP 时，将以下三行添加到 /etc/iscsi/iscsid.conf。

```
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = user
discovery.sendtargets.auth.password = secret
```

`discovery.sendtargets.auth.username` 的用户在 iSCSI 共享的门户使用的授权访问中设置。 同样，用于 `discovery.sendtargets.auth.password` 的密码是授权访问密码。 如果没有这些行，`iscsiadm` 将不会发现使用 CHAP 身份验证方法的 Portal。

接下来，输入 `sudo iscsiadm \--mode node \--targetname {BASENAME}:{TARGETNAME} \--portal {IPADDRESS} \--login`，其中 `{BASENAME}` 和 `{TARGETNAME}` 是来自发现命令的信息。

![](https://www.truenas.com/docs/images/CORE/LinuxISCSILogin.png)

#### 为iSCSI 磁盘分区

当 iSCSI 共享登录成功时，通过 iSCSI 共享的设备在 Linux 系统上显示为 iSCSI 磁盘。 要查看 Linux 中已连接磁盘的列表，请输入 `sudo fdisk -l`。

![](https://www.truenas.com/docs/images/CORE/FDiskList.png)

由于连接的 iSCSI 磁盘是原始磁盘，因此您必须对其进行分区。 识别列表中的 iSCSI 设备并输入 `sudo fdisk {/PATH/TO/iSCSIDEVICE}`。

![](https://www.truenas.com/docs/images/CORE/FDiskPartition.png)

Shell 在 `sudo fdisk -l` 输出中列出了 iSCSI 设备路径。 对磁盘进行分区时使用 fdisk 命令默认值。

完成磁盘分区后，请记住键入 `w` `w` 命令告诉 `fdisk` 在退出之前保存任何更改。

![](https://www.truenas.com/docs/images/CORE/LinuxISCSIFilesystemCreated.png)

在 iSCSI 磁盘上创建分区后，设备名称上会显示一个分区片。 例如，/dev/sdb1。 输入 `fdisk -l` 以查看新的分区片。

#### 在 iSCSI 磁盘上创建文件系统

最后，使用 mkfs 在设备的新分区片上创建文件系统。 要创建默认文件系统 (ext2)，请输入 `sudo mkfs {/PATH/TO/iSCSIDEVICEPARTITIONSLICE}`。

![](https://www.truenas.com/docs/images/CORE/LinuxISCSIFilesystem.png)

#### 挂载 iSCSI 设备

现在 iSCSI 设备可以挂载和共享数据。 输入 `sudo mount {/PATH/TO/iSCSIDEVICEPARTITIONSLICE}`。 例如，`sudo mount /dev/sdb1 /mnt` 将 iSCSI 设备 sdb1 挂载到 /mnt。

### Windows

要访问 iSCSI 共享上的数据，客户端需要使用 iSCSI发起程序（**iSCSI Initiator**） 。 Windows 7 到 10 Pro 和 Windows Server 2008、2012 和 2019 中预装了 iSCSI Initiator 客户端。通常需要 Windows 专业版。

首先，单击开始菜单并搜索  iSCSI发起程序（**iSCSI Initiator**）。

部分情况下，当iSCSI服务尚未运行时，程序会要求你启动服务，这里单击是（**Yes**）

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIInitiatorApp.png)

接下来，转到配置（**Congiguration**）选项卡并单击更改将 iSCSI 发起程序名称更改为之前创建的相同名称。 单击确定。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIInitiatorConfigName.png)

接下来，切换到 发现（**Discovery**） 选项卡，单击发现门户（**Discover Portal**），然后输入 TrueNAS IP 地址。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIInitiatorDiscoverPortal.png)

转至目标（**Targets**）选项卡，突出显示 iSCSI 目标，然后单击连接（**Connect**）。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIInitiatorTargetConnect.png)

Windows 连接到 iSCSI 目标后，您可以对驱动器进行分区。

搜索并打开磁盘管理（**Disk Management**）应用程序。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIDiskManagementApp.png)

您的驱动器当前应该未分配。 右键单击驱动器，然后单击新建简单卷...（**New Simple Volume…**.）。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIDiskNewVolume.png)

完成向导以格式化驱动器并分配驱动器号和名称。

![](https://www.truenas.com/docs/images/CORE/WindowsISCSIDiskNewVolumeOptions.png)

最后，在文件资源管理器中转到此电脑或我的电脑。 新的 iSCSI 卷应显示在驱动器列表下。 您现在应该能够添加、删除和修改 iSCSI 驱动器上的文件和文件夹。

![](https://www.truenas.com/docs/images/CORE/WindowsiSCSIVolumeLocation.png)

## 扩容 LUNs

TrueNAS 允许用户扩展 Zvol 和基于文件的 LUN，以增加 iSCSI 共享的可用存储空间。

### ZFS卷 LUN（Zvol LUN）

要展开 Zvol LUN，请转至存储（**Storage**） > 存储池（**Pools**）并单击 Zvol LUN 旁边的三个小点（选项），然后选择编辑 Zvol（**Edit Zvol**）。

![](https://www.truenas.com/docs/images/CORE/ExpandingZvolLUNList.png)

在此 zvol 的大小（**Size**）字段中输入新大小，然后单击保存。

![](https://www.truenas.com/docs/images/CORE/ExpandingZvolLUNSize.png)

为防止数据丢失，Web 界面不允许用户减小 Zvol 的大小。 TrueNAS 也不允许用户将 Zvol 的大小增加到存储池大小的 80% 以上。

### 文件LUN（File LUN）

要扩展基于文件的 LUN，您需要知道文件的路径。 您可以通过转至共享 > 块共享 (iSCSI) 并单击范围选项卡来找到路径。 单击基于文件的 LUN 旁边的三个小点（选项） 并选择编辑（**Edit**）。

![](https://www.truenas.com/docs/images/CORE/ExpandingFileLUNPath.png)

突出显示并复制路径，然后单击取消（**CANCEL**）

转到 Shell 并输入 `truncate -s +[size] [path to file]`，然后按 Enter。

`[size]` 是您想要增加文件的空间大小，`[path to file]` 是您之前复制的文件路径。

![](https://www.truenas.com/docs/images/CORE/ExpandingFileLUNShell.png)

命令的示例可能如下所示：`truncate -s +2g /mnt/Shares/Dataset1/FileLun/FileLUN`

最后，返回到共享（**Sharing**） > 块共享 (iSCSI)（**Block Shares (iSCSI)**）中的范围并确保文件大小（**Filesize**）设置为 0，因此共享使用实际的文件大小。

