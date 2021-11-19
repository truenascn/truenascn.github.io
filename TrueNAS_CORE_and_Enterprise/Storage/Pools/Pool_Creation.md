# 创建存储池

TrueNAS 使用 ZFS 数据存储“池”来有效地存储和保护数据。

## 查看存储需求

强烈建议在创建存储池之前查看可用的系统资源并规划存储用例。

* 在存储关键信息时，分配给池的更多驱动器会增加冗余。
* 以冗余或性能为代价最大化可用存储总量意味着分配大容量磁盘并配置池以实现最小冗余。
* 最大化池性能意味着为池安装和分配高速 SSD 驱动器。

在创建池之前，确定您的特定存储要求是一个关键步骤。

## 创建池

要创建新池，请转至存储（**Storage**） > 池（**Pools**）并单击添加（**ADD**）。 选择创建新池（**Create new pool**）并单击创建池（**CREATE POOL**）以打开池管理器（**Pool Manager**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateManager.png)

首先，输入池名称。

加密算法可作为最大限度提高数据安全性的选项。 这也使数据的检索方式变得复杂，并有永久丢失数据的风险！ 有关更多详细信息，请参阅[加密文章](https://www.truenas.com/docs/core/storage/pools/storageencryption/)，并在设置任何加密选项之前确定您是否需要加密。

接下来，配置构成池的虚拟设备 (vdev)。

### 建议布局（ SUGGEST LAYOUT）

单击 **SUGGEST LAYOUT** 允许 TrueNAS 查看所有可用磁盘，并在存储容量和数据冗余之间的平衡配置中使用相同大小的驱动器自动填充主 Data vdev。 要清除建议，请单击重置布局（**RESET LAYOUT**）。

要手动配置池，请根据您的用例添加 vdev。 选中对应磁盘的复选框并单击 -> 将磁盘移动到 vdev。

### vdev类型

池有许多不同类型的 vdev 可用。 这些vdev可用于存储数据或启用池的独特功能：

#### 数据（Data）

用于主存储操作的标准 vdev。 每个存储池至少需要一个 Data vdev。 数据 vdev 配置通常会影响其他类型的 vdev 的配置方式。

##### 复制数据 vdev

单击 重复创建（**REPEAT**） 可以复制带有磁盘的 Data VDev。 当更多磁盘可用且大小相等时，**REPEAT** 按钮会创建另一个具有相同配置的 vdev，称为 vdev 的“镜像”。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateVdevRepeat.png)

当有更多相同大小的磁盘可用时，可以创建原始 vdev 的多个副本。

注意：不要有多个磁盘数量不同的数据 vdev，这会使池功能复杂化并受到限制。

#### 缓存（Cache）

[ZFS L2ARC](https://www.truenas.com/docs/references/l2arc/) 读取缓存与高速存储设备一起使用以加速读取操作。 这可以在创建池后添加或删除。

#### 日志（Log）

提高同步写入速度的 [ZFS LOG](https://www.truenas.com/docs/references/slog/) 设备。 这可以在创建池后添加或删除。

#### 热备（Hot Spare）

为在活动驱动器出现故障时插入 Data vdev 保留的驱动器。 热备件临时用作故障驱动器的替代品，以防止出现更大的池和数据丢失情况。

当故障驱动器更换为新驱动器时，热备件将恢复为非活动状态，并可再次用作热备件。

当故障驱动器仅从池中分离时，临时热备用会提升为完整的 Data vdev 成员，并且不再可用作热备用。

#### 元数据（Metadata）

用于创建[融合池](https://www.truenas.com/docs/core/storage/pools/fusionpool/)以提高元数据和小块 I/O 性能的特殊分配类。

#### 去重（Dedup）

存储 ZFS [数据去重的相关信息](https://www.truenas.com/docs/references/zfsdeduplication/)。 需要为每 X TiB 的一般存储分配 X GiB。 示例：每 1 TiB 数据 vdev 可用性需要 1 GiB 的 Dedup vdev 容量。

要在池创建期间添加不同的 vdev 类型，请单击添加 vdev （**ADD VDEV**）并选择类型。 从可用磁盘`Available Disks`中选择磁盘，然后使用新 vdev 旁边的向右箭头将其添加到该池中。

### vdev布局

根据特定的池用途，添加到 vdev 的磁盘以不同的布局排列。

不支持将具有不同布局的多个 vdev 添加到池中。 当需要不同的 vdev 布局时，请创建一个新池。 

#### 条带（Stripe）

所有的磁盘将都被用于存储数据。 至少需要一个磁盘，并且没有数据冗余。 永远不要使用 Stripe 来存储关键数据！ 单个磁盘故障会导致 vdev 中的所有数据丢失。

#### 镜像（mirror）

每个磁盘中的数据是相同的。 至少需要两个磁盘，具有最多的冗余和最少的容量。

#### 磁盘阵列-ZFS-1型（RAIDZ1)

使用一个磁盘进行奇偶校验，而所有其他磁盘存储数据。 至少需要三个磁盘。

#### 磁盘阵列-ZFS-2型（RAIDZ1)

使用两个磁盘进行奇偶校验，而所有其他磁盘存储数据。 至少需要四个磁盘。

#### 磁盘阵列-ZFS-3型（RAIDZ1)

使用三个磁盘进行奇偶校验，而所有其他磁盘存储数据。 至少需要五个磁盘。

池管理器（**Pool Manager**）根据添加到 vdev 的磁盘数量建议 vdev 布局。 例如，如果添加了两个磁盘，TrueNAS 会自动将 vdev 配置为镜像，其中总可用存储空间是添加的一个磁盘的大小，而另一个磁盘提供冗余。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateMirror.png)

要更改 vdev 布局，请打开 Data VDevs 列表并选择所需的布局。

