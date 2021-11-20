# 替换磁盘

硬盘驱动器或固态驱动器 (SSD) 的使用寿命有限，可能会出现意外故障。 当条带 (RAID0) 池中的磁盘出现故障时，必须重新创建整个池并从备份中恢复所有数据。 始终建议创建具有磁盘冗余的非条带存储池。

为防止进一步丢失冗余或最终丢失数据，请务必尽快更换故障磁盘！ TrueNAS 拥有将新磁盘集成到池中以将池恢复到完整的功能。

## 更换磁盘

需要另一个相同或更大容量的磁盘来替换故障磁盘。 该磁盘必须安装在 TrueNAS 系统中，并且必须不是现有存储池的一部分。 作为替换过程的代价，替换磁盘上的所有数据都将被擦除。

TrueNAS 仪表板（**Dashboard**）会显示磁盘故障何时会降低池的性能。

![](https://www.truenas.com/docs/images/CORE/12.0/DashboardPoolDegraded.png)

单击池卡上的设置（齿轮图标）转到存储（**Storage**） > 池（**Pools**） > 池状态（**Pool Status**）页面并找到故障磁盘。

### 使故障磁盘脱机

单击故障磁盘的右侧的三个点会显示其他操作。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsStatusDiskFailedOptions.png)

建议在开始更换之前使磁盘脱机。 这会从池中删除设备并可以防止硬盘交接问题。

**Q**：我可以使用出现故障但仍处于活动状态的磁盘吗？

**A**：在某些情况下，未完全发生故障的磁盘可以保持在线状态，以在更换过程中提供额外的冗余。 但是，除非知道故障磁盘的确切情况，否则不建议这样做。 尝试更换严重降级的磁盘而不首先将其脱机会导致更换过程明显变慢。

**Q**：出现了离线失败，应该如何处理？

**A**：如果离线（**Offline**）操作失败并显示“Disk offline failed - no valid replicas”消息，请转至 存储（**Storage**） > 存储池（**Pools**），单击降级池的设置（**settings**），然后选择 清理存储池（**Scrub Pool**）。 清理操作完成后，重新打开池状态（**Pool Status**）页面并尝试再次离线（**Offline**）磁盘。

当磁盘状态显示为脱机时，从机箱中物理移除磁盘。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsStatusOffline.png)

如果替换磁盘还没有被物理添加到系统中，请立即添加。

### 新硬盘上线

在池状态（**Pool Status**）页面中，打开之前已经脱机的磁盘的选项，然后单击替换（**Replace**）

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsStatusDiskReplace.png)

选择新的磁盘，然后单击“替换磁盘”（**Replace Disk**）。 新磁盘必须具有与被替换磁盘相同或更大的容量。 当所选的新磁盘存在分区或数据时，替换会失败。 要**销毁**替换磁盘上的任何数据并允许替换继续，请勾选强制（**Force**）选项。

当磁盘擦除完成且 TrueNAS 开始更换故障磁盘时，池状态（**Pool Status**）会更改以显示正在进行的更换。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsStatusReplaceStart.png)

TrueNAS 会在更换过程中重新同步池数据。 对于具有大量数据的池，这可能需要很长时间。 重新同步完成后，池状态屏幕会更新以显示新磁盘，并且池状态返回到联机。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsStatusReplaceComplete.png)

