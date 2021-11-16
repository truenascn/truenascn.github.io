# 安装TrueNAS

[下载](https://www.truenas.com/download-truenas-core/)好.iso文件后，您可以开始安装 TrueNAS！

## ISO验证

通过对ISO文件进行验证，您可以确保您得到的ISO文件完整且未被篡改。

iXsystems 安全团队对 TrueNAS ISO 文件进行加密签名，以便用户可以验证其下载文件的完整性。 本节演示如何使用 [Pretty Good Privacy (PGP)](https://tools.ietf.org/html/rfc4880) 和 [SHA256](https://tools.ietf.org/html/rfc6234) 两种方法验证 ISO 文件。

### PGP ISO 验证

对于这种 ISO 验证方法，您将需要 OpenPGP 加密应用程序。 有许多不同的免费应用程序可用，但 OpenPGP 小组在 https://www.openpgp.org/software/ 上提供了适用于不同操作系统的可用软件列表。 本节中的示例显示在命令提示符下使用 [gnupg2](https://gnupg.org/software/index.html) 验证 TrueNAS .iso，但 [Gpg4win](https://www.gpg4win.org/) 也是 Windows 用户的不错选择。

要验证 .iso 源，请转至 https://www.truenas.com/download-tn-core/ ，展开安全选项，然后单击 PGP 签名以下载 Gnu Privacy Guard (.gpg) 签名文件。 打开 PGP 公钥链接并记下浏览器中的地址和 string 的搜索结果。

使用上述 OpenPGP 加密工具之一导入公钥并验证 PGP 签名。

转到 .iso 和 .iso.gpg 下载位置并使用密钥服务器地址和搜索结果字符串导入公钥：

`q5sys@athena /tmp>  gpg --keyserver keys.gnupg.net --recv-keys 0xc8d62def767c1db0dff4e6ec358eaa9112cf7946
gpg: requesting key 12CF7946 from hkp server keys.gnupg.net
gpg: key 12CF7946: "IX SecTeam <security-officer@ixsystems.com>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
q5sys@athena /tmp>`

使用 `gpg --verify` 比较 .iso 和 .iso.gpg 文件：

`q5sys@athena /tmp>  gpg --verify TrueNAS-12.0-BETA2.1.iso.gpg TrueNAS-12.0-BETA2.iso
gpg: Signature made Thu Aug 27 10:06:02 2020 EDT using RSA key ID 12CF7946
gpg: Good signature from "IX SecTeam <security-officer@ixsystems.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: C8D6 2DEF 767C 1DB0 DFF4  E6EC 358E AA91 12CF 7946
q5sys@athena /tmp>`

此响应意味着签名是正确的，但仍然不受信任。 返回打开 PGP 公钥的浏览器页面，手动确认该密钥是 2019 年 10 月 15 日为 `IX SecTeam <security-officer@ixsystems.com>`（iX 安全团队）颁发的，并已由 iXsystems 帐户签名。

### SHA256 验证

验证校验和的命令因操作系统而异：

- BSD: `sha256 isofile`
- Linux: `sha256sum isofile`
- Mac: `shasum -a 256 isofile`
- Windows or Mac: 用户可以安装其他实用程序，如 [HashCalc](https://hashcalc.soft112.com/) 或 [HashTab](https://implbits.com/products/hashtab/)。

运行该命令产生的值必须与 sha256.txt 文件中显示的值相匹配。 不同的校验和值表示不应使用已损坏的安装程序文件。

## 升级

当系统已经安装了 TrueNAS 时，可以使用较新的安装文件重复安装过程。 这用于[主要版本升级](https://www.truenas.com/docs/core/system/update/)

。

选择安装类型：

## 一、将TrueNAS安装在物理机上

TrueNAS 非常灵活，可以在大多数 x86 计算机上运行。 但是，在构建 NAS 时有许多不同的硬件考虑因素！ 如果您仍在研究 TrueNAS 使用哪种硬件，请阅读非常详细的 [CORE 硬件指南](https://www.truenas.com/docs/core/gettingstarted/corehardwareguide/)。

### 准备安装文件

物理硬件通常需要将 TrueNAS 安装程序刻录到物理设备上。 通常使用 CD 或可移动 USB 设备。 此设备临时连接到系统，用于将 TrueNAS 安装到系统的永久启动设备。

当系统具有可用的 IPMI 并且可以使用本地存储的 .iso 创建虚拟媒体 CD-ROM 时，也可以使用远程安装。

将安装程序写入设备的方法因操作系统而异。

##### 光盘

要使用光盘安装系统，请下载您最喜欢的光盘刻录实用程序并将 .iso 文件刻录到光盘。 将光盘插入 TrueNAS 系统并从光驱启动。

##### Windows

要将 TrueNAS 安装程序写入 Windows 上的 U盘，请将 U盘插入系统并使用 Rufus 之类的程序将 .iso 文件写入记忆棒。 当 Rufus 提示使用哪种写入方法时，请确保选择了 dd 模式。

在 TrueNAS 安装程序写入 U 盘后，Windows 无法识别 U 盘。 要在安装 TrueNAS 后回收 U 盘，请使用 Rufus 编写“不可启动”映像，然后移除并重新插入 U 盘。

##### Linux

要将 TrueNAS 安装程序写入 Linux 上的 U 盘，请将 U 盘插入系统并打开终端。

首先确保 U盘连接路径正确。 在 Linux 中有很多方法可以做到这一点，但一个快速的选择是输入 `lsblk -po +vendor,model` 并记下 U 盘的路径。 这显示在 lsblk 输出的 **NAME** 列中。

接下来，使用 dd 将安装程序写入 U 盘。

**使用 dd 时要非常小心，因为选择错误的 of= 设备路径会导致无法挽回的数据丢失！**

在 CLI 中输入 `dd status=progress if=path/to/.iso of=path/to/USB`。 如果这导致“权限被拒绝”错误，请使用具有相同参数的 `sudo dd` 并输入管理员密码。

##### 远程安装

具有 IPMI 连接的系统，如 TrueNAS Mini，可以使用带有 .iso 的虚拟媒体功能来创建用于安装的虚拟引导设备。 将 .iso 安装在虚拟 CD-ROM 中，允许通过控制台远程安装或更新服务器。

以下是使用 超微IPMI 设置虚拟 CD-ROM 的示例：

1、从**虚拟媒体**菜单中，选择 CD-ROM 映像。
2、填写详细信息：
    1、**共享主机**：存储 .iso 的系统的 IP 地址。
    2、**镜像路径**：图像文件的路径。 示例：install/iso/SCALEAngelfish.iso
3、单击**安装**。
4、单击**刷新状态**并确认正在模拟磁盘。
5、单击**保存**。

### 安装过程

将安装程序添加到设备后，您现在可以将 TrueNAS 安装到所需的硬件系统上。 插入安装介质，或使用 IPMI 加载 iso，然后重新引导或引导系统。 在主板启动屏幕上，使用主板制造商定义的热键启动到主板 UEFI/BIOS。

选择以 UEFI 模式或传统 CSM/BIOS 模式启动。 安装 TrueNAS 时，请为安装做出匹配的选择。 对于 2020 年或之后制造的英特尔芯片组，UEFI 可能是唯一的选择。

如果您的系统支持 SecureBoot，您需要禁用它或将其设置为“其他操作系统”才能启动安装媒体。

选择安装设备作为引导驱动器，退出并重新引导系统。 如果U盘未显示为引导选项，请尝试使用其他 USB 插槽。 哪些插槽可用于引导因硬件而异。

系统引导至安装程序后，请按照以下步骤操作。

选择安装/升级。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallMainScreen.png)

选择所需的安装驱动器。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallDriveScreen.png)

选择*Yes*

![](https://www.truenas.com/docs/images/CORE/12.0/InstallWarningScreen.png)

选择全新安装，表示对下载的 TrueNAS 版本进行全新安装。 **这将擦除所选驱动器的内容。！**

![](https://www.truenas.com/docs/images/CORE/12.0/InstallWarningScreen.png)

当用于安装TrueNAS操作系统的启动设备有足够的额外空间时，您可以选择为交换分区分配一些空间以提高性能。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallPartitionScreen.png)

输入 root 用户的密码，这个密码将用于安装完成之后登录 Web 界面。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallPasswordScreen.png)

执行安装步骤后，重新启动系统并移除安装介质。

恭喜，TrueNAS 现已成功安装！

下一步是[登录 Web 界面](https://www.truenas.com/docs/core/gettingstarted/loggingin/)并开始配置系统。

#### 故障排除

如果无法启动到 TrueNAS安装程序，可以检查以下几项来：

- 检查系统 BIOS 并查看是否有将 USB 仿真从 CD/DVD/软盘更改为硬盘驱动器的选项。 如果仍然无法启动，请检查卡/驱动器是否符合 UDMA。
- 如果系统 BIOS 不支持带 BIOS 仿真的 EFI，请查看它是否有使用传统 BIOS 模式引导的选项。
- 如果系统开始启动但挂起并显示以下重复错误消息：`run_interrupt_driven_hooks: still waiting after 60 seconds for xpt_config`，请进入系统 BIOS 并查找“1394 控制器”的板载设备配置。 如果存在，请禁用该设备并再次尝试启动。
- 如果刻录的映像无法启动并且映像是使用 Windows 系统刻录的，请在尝试使用 [Active@ KillDisk](https://www.killdisk.com/eraser.html) 等实用程序进行第二次刻录之前擦除 U 盘 ）。 否则，第二次刻录尝试将失败，因为 Windows 无法识别从映像文件写入的分区。 使用擦除实用程序时，请务必小心指定正确的 USB 记忆棒！

## 二、将TrueNAS安装在虚拟机上

由于 TrueNAS 是作为 .iso 文件构建和提供的，因此它适用于所有虚拟机解决方案（VMware、VirtualBox、Citrix Hypervisor 等）。 本节演示在 Windows 上使用 VMware Workstation Player 进行安装。

### 最低虚拟机设置

无论虚拟化应用程序如何，请使用以下最低设置：

- 内存: 至少 8192MB (8GB)
- 磁盘: 一个至少 8GB 的虚拟磁盘用于操作系统和引导环境，以及至少一个额外的至少 4GB 的虚拟磁盘用作数据存储。
- 网络: 根据您的主机网络配置使用 NAT、桥接或仅主机。

### ESXi上的FreeBSD UEFI Bug

**VMWare 产品和 EFI 引导模式：**第三方错误目前影响 VMWare 产品上的 EFI (UEFI) 引导。 TrueNAS 应在 BIOS 模式下安装，直到此问题得到解决。 请参阅 [VMware 的文章](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.install.doc/GUID-D1BD27AB-C432-454D-9B2B-DC04E7BA9979.html)。

### VMware 的网络检查

在 VMware VM 中安装 TrueNAS 时，请仔细检查虚拟交换机和 VMware 端口组。 TrueNAS VM 内的插件或jail 的网络连接错误可能是由错误配置的虚拟交换机或VMware 端口组引起的。 确保首先在交换机上启用 MAC 欺骗和混杂模式，然后是 VM 使用的端口组。

#### Jail网络

如果您在 VMware 中安装了 TrueNAS，您将需要功能性网络来创建Jail。

要使 jail 具有功能性网络，您必须更改 VMware 设置以允许混杂、MAC 地址更改和伪造传输。

| 设置         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| 混杂模式     | 在虚拟交换层启用时，所有端口组中定义的对象可以接收 vSwitch 上的所有传入流量。 |
| MAC 地址更改 | 设置为接受时，ESXi 将接受 MAC 地址与初始 MAC 地址不同的地址的请求。 |
| 伪传输       | 设置为接受时，ESXi 不会比较源 MAC 地址和有效 MAC 地址。      |

### 通用虚拟机创建过程

对于大多数管理程序，创建 TrueNAS VM 的过程是相同的：

1、像往常一样创建一个新的虚拟机，注意以下设置。
2、虚拟硬件有一个可引导的 CD/DVD 设备，指向 TrueNAS 安装程序映像（通常是 .iso）。
3、虚拟网卡已配置为可以从您的网络访问。桥接模式是最佳的，因为这会将网卡视为插入现有网络上的简单交换机。
4、某些产品需要识别安装在 VM 上的操作系统。理想的选择是 FreeBSD 12 64 位。如果这不可用，请尝试 FreeBSD 12、FreeBSD 64   		位、64 位操作系统或其他等选项。不要选择与 Windows 或 Linux 相关的操作系统类型。
5、对于 VMWare 管理程序，请在 BIOS 模式下安装。
6、VM 有足够的内存和磁盘空间。 TrueNAS 至少需要 8 GB RAM 和 20 GB 磁盘空间。默认情况下，并非所有管理程序都分配足够的内		存。
7、启动虚拟机并像往常一样安装 TrueNAS。
8、安装完成后，关闭虚拟机而不是重新启动，并在重新启动虚拟机之前断开 CD/DVD 与虚拟机的连接。
9、重新启动到 TrueNAS 后，如果适用于您的 VM，并且它们存在于 FreeBSD 12，请安装 VM 工具，或确保它们在启动时加载。

### VMWare Player 15.5 的安装示例

打开VMware Player，点击新建虚拟机，进入新建虚拟机向导。

**1、安装盘镜像文件**
选择安装镜像文件 (iso)选项，点击浏览...，上传之前下载的TrueNAS Core .iso。

**2、命名虚拟机**
在这一步中，可以更改虚拟机名称和位置。

**3、指定磁盘容量**
指定初始磁盘的最大磁盘大小。 TrueNAS 默认的 20GB 就足够了。 接下来，选择将虚拟磁盘存储为单个文件。

**4、查看虚拟机**
在继续之前检查虚拟机配置。 默认情况下，VMware Player 不会为虚拟机设置足够的 RAM。 单击自定义硬件... > 内存。 将滑块拖动到 8GB，然后单击确定。 如果您希望在创建后打开计算机电源，请选择创建后打开此虚拟机电源。

### 为存储添加虚拟磁盘

创建虚拟机后，从虚拟机列表中选择它，然后单击编辑虚拟机设置。 单击添加...并选择硬盘。 选择 SCSI 作为虚拟磁盘类型。 选择创建新的虚拟磁盘。 指定此附加虚拟磁盘的最大大小。 该磁盘将数据存储在 TrueNAS 中。 如果需要，通过设置立即分配所有磁盘空间来立即分配磁盘空间。 选择将虚拟磁盘存储为单个文件。 最后，为新虚拟磁盘命名并选择一个位置。

重复此过程，直到有足够的磁盘可供 TrueNAS 使用以创建理想的存储池。这取决于您的特定 TrueNAS 用例。 有关各种池 (“vdev”) 类型和布局的说明，请参阅池创建

### TrueNAS 安装

从列表中选择虚拟机，然后单击播放虚拟机。 机器启动并引导至 TrueNAS 安装程序。 选择安装/升级。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallVMMainScreen.png)

为引导环境选择所需的磁盘。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallVMMediaScreen.png)

选择是。 **这将擦除虚拟磁盘上的所有内容！**

![](https://www.truenas.com/docs/images/CORE/12.0/InstallVMWarningScreen.png)

设置root登录密码。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallVMPasswordScreen.png)

选择通过 BIOS 启动。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallVMBootMode.png)

TrueNAS安装完成后，重启系统。 当系统成功引导时，将显示[控制台设置菜单](https://www.truenas.com/docs/core/gettingstarted/consolesetupmenu/)。

### VMWare 安装后

在 VMware VM 中安装 TrueNAS 后，建议在 TrueNAS 上配置和使用 vmx(4) 驱动程序。 要在 TrueNAS 启动时加载 VMX 驱动程序，请登录 Web 界面并转到系统(System) > 可调参数(Tunables)。 单击添加并使用变量(Variable) if_vmx_load、值(Value)“YES”和类型加载器(Type loader)创建新的可调参数，然后保存可调参数：

![](https://www.truenas.com/docs/images/CORE/12.0/SystemTunablesVmxload.png)

恭喜，TrueNAS 现已成功安装！

下一步是[登录 Web 界面](https://www.truenas.com/docs/core/gettingstarted/loggingin/)并开始配置系统。

