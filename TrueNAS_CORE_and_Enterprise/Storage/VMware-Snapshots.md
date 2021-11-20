# VMware虚拟机快照

当使用 TrueNAS 为 VMware 数据存储时，存储（**Storage**） > VMware 快照（**VMware-Snapshots**）协调 ZFS 快照。创建 ZFS 快照后，TrueNAS 会在拍摄数据集的计划或手动 ZFS 快照或支持该 VMware 数据存储的 zvol 之前自动对任何正在运行的 VMware 虚拟机进行快照。

**必须打开虚拟机电源**才能将 TrueNAS 快照复制到 VMware。临时 VMware 快照随后会在 VMware 端删除，但仍存在于 ZFS 快照中并可用作稳定还原点。这些协调的快照位于存储（**Storage**） > 快照（**Snapshots**）列表中。

您需要购买 VMware ESXi 的付费版本才能使用 VMware-Snapshots。如果您尝试将它们与 ESXi free 一起使用，则会看到以下错误消息：“错误：无法创建快照，当前许可证或 ESXi 版本禁止执行请求的操作。”（**“Error: Can’t create snapshot, current license or ESXi version prohibits execution of the requested operation.”**）。事实上，ESXi free（免费版） 已锁定（只读）API，阻止使用 TrueNAS VMware-Snapshots。与 TrueNAS VMware-Snapshots 兼容的最便宜的 ESXi 版本是 VMware vSphere Essentials Kit。

## 创建 VMware 快照

转至存储（**Storage**） > VMware 快照（**VMware-Snapshots**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageVMwareSnapshotsAdd.png)

| 设置选项                          | 值                       | 描述                                                         |
| --------------------------------- | ------------------------ | ------------------------------------------------------------ |
| 主机名（**Hostname**）            | 字符串/string            | 输入 VMware 主机的 IP 地址或主机名。 集群时，使用集群的 vCenter 服务器的 IP 地址或主机名。 |
| 用户名（**Username**）            | 字符串/string            | 输入在 VMware 主机上创建的用户帐户名称。 该帐户必须具有对虚拟机进行快照的权限。 |
| 密码（**Password**）              | 字符串/string            | 输入与用户名（**Username**）关联的密码。                     |
| ZFS文件系统（**ZFS Filesystem**） | 浏览……按钮/browse button | 选择要快照的文件系统。                                       |
| 数据存储（**Datastore**）         | 下拉菜单/drop-down menu  | 输入 主机名（**Hostname**）、用户名（**Username**） 和 密码（**Password**）后，单击获取数据存储（**FETCH DATASTORES**） 以填充菜单。 选择要同步的数据存储。 |

单击获取数据存储（**FETCH DATASTORES**）后，TrueNAS 会连接到 VMware 主机。 ZFS 文件系统和数据存储下拉菜单从 VMware 主机响应中填充。 选择数据存储也会选择先前映射的数据集。

