# 融合池

融合池也称为 ZFS 分配类（**ZFS Allocation Classes**）、ZFS 特殊 vdev（**ZFS Special vdevs**） 和元数据 vdev（**Metadata vdevs**）。

**Q**：什么是ZFS 特殊 vdev（**ZFS Special vdevs**）？

**A**：一个特殊 vdev 可以存储元数据，例如文件位置和分配表。 特殊类中的分配专用于特定的块类型。 默认情况下，这包括所有元数据、用户数据的间接块和任何重复数据删除表。 该类还可以设置为接受小文件块。 这是高性能但尺寸较小的固态存储的绝佳用例。 使用特殊的 vdev 极大地加快了随机 I/O，并将查找和访问文件所需的平均旋转磁盘 I/O 减少了一半。

## 创建融合池

转至存储（**Storage**） > 池（**Pools**），单击添加（**ADD**），然后选择创建新池（**Create new pool**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateManager.png)

在可以将其他设备分配给特殊类之前，池必须始终具有一个正常（非重复数据删除/特殊）vdev。 先配置数据 VDev（**Data VDevs**），然后单击添加 VDEV（**ADD VDEV**） 并选择元数据（**Metadata**）。

将固态硬盘（SSDs）添加到新的元数据Vdev（ **Metadata VDev**） 并选择与数据 VDev（**Data VDevs**） 相同的布局。

使用带有内部缓存的 SSD 时，向系统添加不间断电源 (UPS) 以帮助最大程度地降低断电风险。

可以使用镜像（Mirror）布局，但强烈建议保持布局与其他 vdev 相同。 如果特殊 vdev 出现故障并且没有冗余，则存储池会损坏并无法访问存储的数据。

注意：添加到元数据 vdev 的驱动器无法从池中删除。

当创建了多个元数据 vdev 时，分配会在所有这些设备之间进行负载平衡。 如果特殊类vdev已满，则分配会溢出到普通类中。

创建融合池后，状态（**Status**）会显示带有元数据 SSD 的特殊部分。 请参阅[管理存储池](https://www.truenas.com/docs/core/storage/pools/managingpools/)。