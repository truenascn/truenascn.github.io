# UNIX 共享 (NFS)

在 TrueNAS 上创建网络文件系统 (NFS) （**Network File System (NFS) **）共享的好处是可以让任何具有共享访问权限的人轻松访问大量数据。 根据共享的配置方式，可以将访问共享的用户限制为读取或写入权限。

要创建新共享，请确保存在一个包含需要共享的数据的数据集。

## 创建 NFS 共享

转至共享（**Sharing**） > Unix 共享 (NFS)（**Unix Shares (NFS)**），然后单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingNFSAdd.png)

使用文件浏览器选择要共享的数据集。 可以输入可选的描述来帮助识别共享。 单击提交（**SUBMIT**）创建共享。 在创建时，您可以选择 启动服务（**ENABLE SERVICE**） 以启动服务并在重新启动后自动启动。 如果您希望创建共享但不立即启用它，请选择取消（**CANCEL**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingNFSAddServiceEnable.png)

![](https://www.truenas.com/docs/images/CORE/12.0/SharingNFSAddServiceEnableSuccess.png)

### NFS 共享设置

| 设置选项                   | 值                      | 描述                                                         |      |
| -------------------------- | ----------------------- | ------------------------------------------------------------ | ---- |
| 路径（**Path**）           | 文件浏览器/file browser | 键入或浏览到要共享的池或数据集的完整路径。 点击添加（**ADD**）配置多条路径。 |      |
| 描述（**Description**）    | 字符串/string           | 输入有关共享的任何注释或提醒。                               |      |
| 包含子目录（**All dirs**） | 复选框/checkbox         | 设置为允许客户端挂载 路径（**Path**） 内的任何子目录。 保持禁用仅允许客户端挂载 路径（**Path**） 端点。 |      |
| 静默共享（**Quiet**）      | 复选框/checkbox         | 启用会禁止某些系统日志诊断以避免出现错误消息。 有关示例，请参阅 [exports(5)](https://www.freebsd.org/cgi/man.cgi?query=exports)。 取消勾选会允许所有 syslog 诊断，这可能会导致额外的外观错误消息。 |      |
| 启用（**Enabled**）        | 复选框/checkbox         | 启用此 NFS 共享。 取消设置以禁用此 NFS 共享而不删除配置。    |      |

### 高级选项

打开 高级选项（**ADVANCED OPTIONS**） 允许调整共享访问权限和定义授权网络。

![](https://www.truenas.com/docs/images/CORE/12.0/SharingNFSAddAdvanced.png)

| 设置选项                                                  | 值                                   | 描述                                                         |
| --------------------------------------------------------- | ------------------------------------ | ------------------------------------------------------------ |
| 只读（**Read Only**）                                     | 复选框/checkbox                      | 勾选时禁止写入共享。                                         |
| 映射根用户（**Maproot User**）                            | 字符串或下拉菜单/string or drop down | 选择一个用户以将该用户的权限应用于 *root* 用户。             |
| 映射根组（**Maproot Group**）                             | 字符串或下拉菜单/string or drop down | 选择一个组以将该组的权限应用于 *root* 用户。                 |
| 映射所有用户（**Mapall User**）                           | 字符串或下拉菜单/string or drop down | 所选用户的权限适用于所有客户端。                             |
| 映射所有组（**Mapall Group**）                            | 字符串或下拉菜单/string or drop down | 所选组的权限适用于所有客户端。                               |
| 授权网络（**Authorized Networks**）                       | IP地址/IP address                    | 以网络/掩码 CIDR 表示法输入允许的网络。 单击添加（**ADD**） 定义另一个授权网络。 定义授权网络会限制对所有其他网络的访问。 留空以允许所有网络。 |
| 授权主机和IP地址（**Authorized Hosts and IP addresses**） | 字符串/string                        | 输入主机名或 IP 地址以允许该系统访问 NFS 共享。 单击添加（**ADD**）定义另一个允许的系统。 定义授权系统会限制对所有其他系统的访问。 将字段留空以允许所有系统访问共享。 |

要编辑现有的 NFS 共享，请转至共享（**Sharing**） > Unix 共享 (NFS)（**Unix Shares (NFS)**），然后单击右侧的三个小点（选项） > 编辑（**Edit**）。 可以修改的选项与共享创建时的选项相同。

## 配置 NFS 服务

要开始共享数据，请转到服务（**Services**）并单击 NFS 切换开关。 如果您希望在 TrueNAS 启动后立即激活 NFS 共享，请勾选自动启动（**Start Automatically**）。

NFS 服务设置可以通过单击配置图标进行配置。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesNFSOptions.png)

| 设置选项                                                     | 值                 | 描述                                                         |
| ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| 服务器数量（**Number of servers**）                          | 整数/integer       | 指定要创建的服务器数量。 如果 NFS 客户端响应缓慢，则增加。 保持它小于或等于 `sysctl -n kern.smp.cpus` 报告的 CPU 数量以限制 CPU 前后切换。 |
| IP绑定（**Bind IP Addresses**）                              | 下拉菜单/drop down | 选择要侦听 NFS 请求的 IP 地址。 为 NFS 留空以侦听所有可用地址。 |
| 启用NFSv4（**Enable NFSv4**）                                | 复选框/checkbox    | 设置为从 NFSv3 切换到 NFSv4。                                |
| NFSv4 的 NFSv3 所有权模型（**NFSv3 ownership model for NFSv4**） | 复选框/checkbox    | 在不需要客户端和服务器同步用户和组的情况下需要 NFSv4 ACL 支持时设置。 |
| 需要 Kerberos的NFSv4 （**Require Kerberos for NFSv4**）      | 复选框/checkbox    | 如果 Kerberos 票证不可用，则NFS 共享会失败。                 |
| 服务器 UDP NFS 客户端（**Serve UDP NFS clients**）           | 复选框/checkbox    | 设置 NFS 客户端是否需要使用用户数据报协议 (UDP)。            |
| 允许非root挂载（**Allow non-root mount**）                   | 复选框/checkbox    | 仅在 NFS 客户端需要时设置。 设置为允许非 root 用户的挂载请求。 |
| 支持多于16个组（**Support >16 groups**）                     | 复选框/checkbox    | 当用户是超过 16 个组的成员时设置。 这假设在 NFS 服务器上正确配置了组成员身份。 |
| 记录 mountd(8) 请求（**Log mountd(8) requests**）            | 复选框/checkbox    | 设置为记录 [mountd](https://www.freebsd.org/cgi/man.cgi?query=mountd) syslog 请求。 |
| 记录 rpc.statd(8) 和 rpc.lockd(8)（**Log rpc.statd(8) and rpc.lockd(8)**） | 复选框/checkbox    | 设置为记录 [rpc.statd](https://www.freebsd.org/cgi/man.cgi?query=rpc.statd) 和 [rpc.lockd](https://www.freebsd.org/cgi/ man.cgi?query=rpc.lockd) 系统日志请求。 |
| mountd(8) 绑定端口（**mountd(8) bind port**）                | 整数/integer       | 输入一个数字以将 [mountd](https://www.freebsd.org/cgi/man.cgi?query=mountd) 绑定到该端口。 |
| rpc.statd(8) 绑定端口（**rpc.statd(8) bind port**）          | 整数/integer       | 输入一个数字以将 [rpc.statd](https://www.freebsd.org/cgi/man.cgi?query=rpc.statd) 仅绑定到该端口。 |
| rpc.lockd(8) 绑定端口（**rpc.lockd(8) bind port**）          | 整数/integer       | 输入一个数字，将 [rpc.lockd](https://www.freebsd.org/cgi/man.cgi?query=rpc.lockd) 仅绑定到该端口。 |

除非需要特定设置，否则建议使用 NFS 服务的默认设置。 当 TrueNAS 已连接到 [Active Directory](https://www.truenas.com/docs/core/directoryservices/activedirectory/) 时，为 NFSv4 设置 NFSv4 和需要 Kerberos 也需要 [Kerberos Keytab](https://www.truenas.com/docs/core/directoryservices/kerberos/#kerberos-keytabs)。

## 连接到 NFS 共享

虽然您可以通过各种操作系统连接到 NFS 共享，但建议使用 Linux/Unix 操作系统。首先，下载nfs-common 内核模块。这可以使用已安装发行版的包管理器来完成。例如，在 Ubuntu/Debian 上，在终端中输入 `sudo apt-get install nfs-common`。

安装模块后，通过输入 `sudo mount -t nfs {IPaddressOfTrueNASsystem}:{path/to/nfsShare} {localMountPoint}` 连接到 NFS 共享。在上面的例子中，`{IPaddressOfTrueNASsystem}`是包含NFS共享的远程TrueNAS系统的IP地址，`{path/to/nfsShare}`是TrueNAS系统上NFS共享的路径，`{localMountPoint}`是本地目录在为挂载的 NFS 共享配置的主机系统上。例如，`sudo mount -t nfs 10.239.15.110:/mnt/pool1/photoDataset /mnt/photos` 会将 NFS 共享 `photoDataset` 挂载到本地目录 `/mnt/photos`。

默认情况下，连接到 NFS 共享的任何人都只有读取权限。要更改默认权限，请编辑共享，打开高级选项（**Advanced Options**），然后更改访问（**Access**）设置。

具有 NFSv4 共享的读/写功能需要 ESXI 6.7 或更高版本。

