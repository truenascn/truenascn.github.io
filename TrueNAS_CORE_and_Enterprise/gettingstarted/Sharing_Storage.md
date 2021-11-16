# TrueNAS存储共享

配置和备份 TrueNAS 存储后，是时候开始共享数据了。 有几种可用的共享解决方案，但我们将在本文中介绍最常见的解决方案。 

## 共享数据

### Windows (SMB)

#### 要求

- 共享类型设置为 SMB 的数据集。
- 设置了 Samba 身份验证的 TrueNAS 用户帐户。

#### 设置权限

转到存储（**Storage**） > 池（**Pools**）并找到要共享的数据集。 单击右侧的三个点展开菜单，点击编辑权限（***Edit Permissions***）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEditACL.png)

单击 （**SELECT AN ACL PRESET**），打开下拉菜单，然后选择 （**OPEN**）。 单击保存（**SAVE**）。

#### 创建共享

转至共享（**Sharing**） > Windows 共享 (SMB)（**Windows Shares (SMB)**），然后单击添加（***ADD***）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAdd.png)

基本共享只需要路径和名称。 Path 是 TrueNAS 上使用 SMB 协议共享的目录树。 当 SMB 客户端连接时，名称构成“完整共享路径名”的一部分。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingSMBAddBasicExample.png)

单击提交以将配置保存到共享 > Windows 共享 (SMB)。

#### 激活服务

转到服务并切换 SMB。 如果您希望在 TrueNAS 启动后立即访问共享，请设置自动启动。

#### 连接到共享

在 Windows 10 系统上，打开资源管理器。

![](https://www.truenas.com/docs/images/CORE/WindowsFileBrowser.png)

在地址栏中，输入 `\` 和 TrueNAS 系统名称。 出现提示时，输入 TrueNAS 用户帐户凭据并开始浏览数据集。

![](https://www.truenas.com/docs/images/CORE/WindowsSMBShareConnected.png)

### 类UNIX（NFS）

#### 要求

- TrueNAS 数据集共享。
- 客户端系统可能需要额外的软件包，如 `nfs-common`。

#### 创建共享

转至共享（**Sharing**） > Unix 共享 (NFS)（**Unix Shares (NFS)**），然后单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingNFSAdd.png)

使用文件浏览器选择要共享的数据集，然后单击提交。 出现提示时，单击启用服务以立即开始共享数据集。

#### 访问数据集

在类 Unix 系统上，打开命令行。 输入 `showmount -e IPADDRESS`（用您的 TrueNAS 系统地址替换 IPADDRESS）：

`Enter an option from 1-12: 1`
`tmoore@ChimaeraPrime:~$ showmount -e 10.238.15.194`
`Export list for 10.238.15.194:`
`/mnt/pool1/testds (everyone)`

现在为 NFS 挂载创建一个本地目录：

`tmoore@ChimaeraPrime:~$ sudo mkdir nfstemp/`

最后，挂载共享目录：

`tmoore@ChimaeraPrime:~$ sudo mount -t nfs 10.238.15.194:/mnt/pool1/testds nfstemp/`

使用cd 进入本地目录并根据需要查看或修改文件。

### 块共享 (iSCSI)

块共享是一个复杂的场景，需要详细的配置步骤和网络环境的知识。 简单的配置超出了本入门指南的范围，但 [iSCSI 共享主题](https://www.truenas.com/docs/core/sharing/iscsi/iscsishare/)中提供了详细的文章

