# 快照

快照是 ZFS 最强大的功能之一。 快照提供文件系统或卷的只读时间点副本。 此副本不会占用 ZFS 池中的额外空间。 每当修改数据时，快照仅记录存储块引用之间的差异。

快照保留文件的历史记录，并提供一种恢复较旧甚至已删除文件的方法。 出于这个原因，许多管理员定期拍摄快照，将它们存储一段时间，然后将它们复制到不同的系统。 此策略允许管理员将系统数据回滚到特定时间点。 如果发生灾难性的系统或磁盘故障，异地快照可以将数据恢复到最近的快照。

拍摄快照前要求系统已配置好所有[池](https://www.truenas.com/docs/core/storage/pools/)、[数据集](https://www.truenas.com/docs/core/storage/snapshots/)和 [zvol](https://www.truenas.com/docs/core/storage/snapshots/)。

## 创建单个快照

建议设置定期快照任务以节省时间并创建定期、新鲜的快照。

要快速创建现有存储的快照，请转至存储（ **Storage**） > 快照（**Snapshots**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageSnapshotsAdd.png)

使用数据集（**Dataset**）下拉列表选择要创建快照的现有 ZFS 池、数据集或 zvol。

TrueNAS 软件会显示一个建议名称，您可以使用任何自定义字符串覆盖该名称。 要将快照包含在复制任务中，请选择适当的命名模式。 **Naming Schema** 下拉列表中填充了已从定期快照任务创建的模式。

要在快照中包含子数据集，请设置 递归（**Recursive**）。

## 管理快照

进入存储（ **Storage**） > 快照（**Snapshots**）管理创建的快照。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageSnapshots.png)

列表中的每个条目都包含数据集和快照名称。 单击 > 查看快照的选项。

### 快照选项

#### 创建日期（DATE CREATED）

创建快照的确切时间和日期。

#### 使用空间（USED）

此数据集及其所有后代数据集占用的空间量。此值根据数据集配额和预留进行检查，显示已使用的空间，但不包括数据集预留。它考虑了任何后代数据集的保留。数据集从其父级消耗的空间量以及递归删除该数据集时释放的空间量是其已用空间和预留空间中的较大者。

创建时，快照在快照、文件系统甚至以前的快照之间共享空间。文件系统更改会减少共享空间并计入快照的已用空间。删除快照通常会增加在其他快照中使用的唯一空间。

查看单个快照使用的空间的另一种方法是进入**Shell**并输入`zfs list -t snapshot`。

已使用、可用或引用的空间不考虑挂起的更改。挂起的更改通常会在几秒钟内更新，但较大的磁盘更改会减慢使用更新。

#### 参照空间（REFERENCED）

此数据集可访问的数据量。 这可以与池中的其他数据集共享。 新快照或克隆引用与创建它的文件系统或快照相同的空间量，因为内容相同。

#### 删除（DELETE）

删除选项会破坏快照。 您必须先删除子克隆，然后才能删除其父快照。 虽然创建快照是即时的，但删除快照是 I/O 密集型操作，可能需要很长时间，尤其是在启用重复数据删除时。因为ZFS 必须在删除之前检查所有已分配的块，以查看是否有另一个进程正在使用该块。 如果不使用，ZFS 可以释放该块。

#### 将快照克隆到新数据集（CLONE TO NEW DATASET）

从快照内容创建新的快照克隆（数据集）。克隆是快照的可写副本。 因为克隆实际上是一个可挂载的数据集，它出现在池屏幕而不是快照屏幕中。 默认情况下，创建新快照名称中会含有 -clone 。

一个对话框提示输入新的数据集名称。 建议的名称源自快照名称。

#### 回滚快照（ROLLBACK）

将数据集恢复到快照保存的时间点的状态。

TrueNAS 在回滚到所选快照状态之前要求确认。 单击是（**Yes**） 会将所有数据集文件恢复到它们在创建快照时所处的状态。

回滚是一个危险的操作，它会导致任何配置的复制任务失败。 执行增量备份时，复制使用现有快照，回滚可能会使快照“无序”。 要恢复快照中的数据，建议的步骤是：

1. 克隆所需的快照。
2. 与在 TrueNAS 系统上运行的共享类型或服务共享克隆。
3. 允许用户恢复他们需要的数据。
4. 从存储（ **Storage**） > 存储池（**Pools**）中删除克隆。

这种方法不会破坏任何磁盘上的数据，也不会影响复制。

### 批量操作

要删除多个快照，请分别选择要包含的每个快照的左列框。 单击显示的删除（**Delete**）按钮。

要按名称搜索快照列表，请在搜索过滤快照（**Filter Snapshots**）文本字段中输入匹配条件。 该列表现在仅显示与过滤器文本匹配的快照名称。

## 浏览快照集合

浏览快照集合是一项高级功能，需要 ZFS 和命令行经验。
所有数据集快照都可以作为普通分层文件系统访问，从位于每个数据集根目录的隐藏 .zfs 访问。

如果快照存放路径超过 88 个字符，则无法访问或搜索快照及其包含的任何文件。 快照中的数据是安全的，但要使快照再次可访问会要求缩短存放路径。

有权访问隐藏文件的用户可以使用 **SMB**、**NFS** 和 **SFTP** 等服务从命令行管理程序或共享屏幕查看和浏览数据集的所有快照。

总之，需要对设置进行的主要更改是：

- 在数据集属性中，更改 ZFS 属性以启用快照可见性。
- 在 Samba 辅助设置中，将 `veto files` 命令更改为不隐藏 .zfs，并添加设置 `zfsacl:expose_snapdir=true`。

效果是任何可以访问数据集内容的用户都可以通过转到数据集 .zfs 目录来查看快照列表。 用户可以在整个数据集快照集合中浏览和搜索他们有权访问的任何文件。

创建快照时，对该快照内的文件设置的权限或 ACL 可能会限制对文件的访问。

快照是只读的，因此用户无权修改快照或其文件，即使他们在创建快照时具有写入权限。

可以在 **Shell** 中运行`zfs diff` 命令，它会列出数据集中任意两个快照版本之间或任意快照与当前数据之间的所有更改文件。

