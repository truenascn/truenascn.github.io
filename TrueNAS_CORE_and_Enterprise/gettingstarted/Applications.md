# TrueNAS应用

在系统的其余部分配置好并且数据通过网络共享后，首次设置要考虑的最后一步是安装一些应用程序解决方案。 添加到 TrueNAS 的应用程序或功能是在与基础 TrueNAS 操作系统分开的单独“插件”（**Plugins**）、**Jail** 或“虚拟机”（**Virtual Machines**）中创建的。如果出现任何问题或在这些应用程序环境中存在安全漏洞， TrueNAS则不受影响，这些解决方案以受保护的方式安全地扩展了 TrueNAS 的功能。

安装应用程序的主要方法是使用插件（**Plugins**）。 这些是预先打包的应用程序，可在量身定制的环境中快速安装。 一些插件由 iXsystems 支持，而另一些则由开源社区提供和维护。

**Jail** 是一个受限的 FreeBSD 操作系统，作为 TrueNAS 的一个独立子集安装。 Jails 可以安装各种各样的应用程序并针对非常特定的用例进行调整，但需要更广泛的 FreeBSD 和命令行操作知识。

虚拟机（**Virtual Machine**）是完全独立的操作系统。 这会保留或拆分可用的硬件资源以创建不同的、完整的操作系统体验。 TrueNAS 可以在虚拟机 (VM) 中安装 Windows 或类 Unix 操作系统，但虚拟机运行时会降低常规系统性能。

| 网络硬件卸载                                                 |
| ------------------------------------------------------------ |
| 使用网络接口的插件需要在网络（**Network**） -> 接口（ **Interface**）中禁用硬件卸载。 禁用硬件卸载会降低该接口的一般网络性能，因此建议对应用程序环境使用辅助接口。 |

## 一、插件

本说明通过引导您安装社区最喜欢的 Plex 应用程序来演示插件。 您需要一个 Plex 帐户才能按照这些说明进行操作。

### 安装PLEX

创建一个名为 audio 的数据集和一个名为 video 的数据集，用作 Plex 的挂载点。 接下来，转到插件（**Plugins**）页面。

安装基本的 PlexMedia 插件：

1、选择 Plex Media Server 插件并单击安装（INSTALL）。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexInstallButton.png)

2、在Jail名称下，输入您喜欢的任何名称（即“Plex”）。
3、DHCP 会自动设置。
4、单击保存（**SAVE**）。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexMediaSave.png)

5、会弹出一个对话窗口显示安装进度。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexInstallProgress.png)

如果可用，安装完成时会显示插件安装说明。

6、插件状态显示为 **up**，并设置了 Boot 选项。
7、单击” >“ 展开 Plex 表条目：

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexJailUp.png)

8、停止 up 插件。
9、单击挂载点（***MOUNT POINTS***）。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexSetMountpoints.png)

10、单击操作（**Actions**），添加（**Add**）。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsMountPointsPlexAddMountpoint.png)

11、为每个先前创建的数据集填写一个挂载点。 Source 是创建的数据集，Destination 是附加了 /datasetname 的媒体目录（参见示例）：

![](https://www.truenas.com/docs/images/CORE/12.0/JailsMountPointsPlexSetMountpoint.png)

12、单击提交（***Submit*.**）。 根据需要对尽可能多的挂载点执行此操作。 在这个例子中，我们有*audio*和*video*。

13、转至存储（**Storage**） > 池（**Pools**），然后单击右边的三个点 > 源数据集的编辑权限（***Edit Permissions***）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsPlexEditPermissions.png)

14、单击创建自定义 ACL 并继续。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsPermissionsPlexACL.png)

15、单击添加 ACL ITEM 并输入下图所示的值：

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsPermissionsPlexPermissions.png)

设置递归应用权限（***Apply permissions recursively***），然后单击保存。

16、转到插件（**Plugins**），找到 **Plex**条目，然后单击 >。 启动插件。

### 访问 PLEX

1、当 Plex 插件状态为 up 时，单击 > 和管理。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexManage.png)

2、输入您的 Plex 登录信息。

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexLogin.png)

![](https://www.truenas.com/docs/images/CORE/12.0/PluginsPlexSuccess.png)

## 二、FreeBSD Jails

### 安装Jail

1、转到 Jails 页面并单击 **ADD**。

![](https://www.truenas.com/docs/images/CORE/12.0/Jails.png)

2、输入 jail Name，选择 Release 版本，然后单击 NEXT。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsAddName.png)

3、要允许jail 访问互联网，请设置*DHCP Autoconfigure IPv4*  并单击NEXT。 设置 DHCP 选项时会设置其他默认值。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsAddNetworkingDHCP.png)

4、查看jail摘要（**Jail Summary**）并单击提交（**SUBMIT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsAddConfirm.png)

### 访问Jail

1、转到 Jails 并单击新创建的 jail 旁边的 >。 单击开始。

![](https://www.truenas.com/docs/images/CORE/12.0/JailsStart.png)

2、当 jail State 变为 up 时，单击 > SHELL 查看 jail 命令行。

![](https://www.truenas.com/docs/images/CORE/12.0/JailShell.png)

## 三、虚拟机

### 安装虚拟机

虚拟机需要将操作系统 .iso 上传到 TrueNAS。 此示例显示使用 Ubuntu .iso：

1、转到虚拟机（**Virtual Machines**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachines.png)

2、选择客户机操作系统并输入名称。 在本示例中，客户操作系统设置为 Linux。 点击下一步。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddOperatingSystemLinux.png)

3、现在输入物理资源给VM。 分配更多的虚拟 CPU、内核、线程和内存会使虚拟机性能更好，但会降低 TrueNAS 宿主机系统的性能。 点击下一步。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddCPU.png)

4、设置创建新磁盘映像并为 VM 存储选择 Zvol 位置。 输入可用的存储大小（示例显示 50 GiB）并单击下一步按钮。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddDisks.png)

5、网络接口（**Network Interface**）自动检测硬件并设置允许网络访问的默认值。 确保这些设置有效，然后单击 NEXT。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddNetworkInterface.png)

6、上传虚拟机系统安装程序映像文件后。在 TrueNAS 系统上选择 ISO 保存位置。而现在则单击选择文件并浏览到操作系统安装 .iso直接上传到TrueNAS中，单击上传并等待该过程完成（这可能需要一些时间）。 点击下一步。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddInstallationMedia.png)

7、确认虚拟机配置正确，点击提交（**SUBMIT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesAddConfirm.png)

### 访问虚拟机

1、转至虚拟机（**Virtual Machines**）并单击新创建的 VM 旁边的 >。 单击开始。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesStart.png)

2、当 **VM State** 变为 **up** 时，单击 VNC 以查看虚拟机的虚拟显示器。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesOptions.png)

由于此示例使用了 Ubuntu .iso，因此显示了 Ubuntu 安装界面。

![](https://www.truenas.com/docs/images/CORE/12.0/UbuntuInstall.png)

从这里开始，像在物理机里安装操作系统一样照常安装。

3、操作系统安装完成后，返回到虚拟机（**Virtual Machines**），切换状态，然后单击设备（**DEVICES**）。

![](https://www.truenas.com/docs/images/CORE/12.0/VirtualMachinesDevices.png)

找到 **CDROM** 条目并单击右侧的三个点 > 点击删除（***Delete***）以将其删除。 这会从虚拟机中删除安装 .iso，并允许虚拟机下次启动到完整的操作系统。

