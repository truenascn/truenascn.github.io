# 擦除磁盘

擦除（**WIPE**） 功能等同于Microsoft Windows中对于磁盘的格式化（**Format**）命令，可以清理未使用的磁盘中的数据。

**注意：这是一种破坏性操作，会导致永久性数据丢失！ 备份要擦除的磁盘上的所有关键数据。**

要擦除磁盘，请转至存储（**Storage**） > 磁盘（**Disks**）。 单击磁盘的 > 以查看所有选项。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageDisksExpand.png)

擦除（**WIPE**） 选项仅在磁盘不被使用时可用。 单击擦除（**WIPE**） 打开一个带有附加选项的对话框：

![](https://www.truenas.com/docs/images/CORE/12.0/StorageDisksWipeMethod.png)

磁盘名称（da1、da2、ada4）有助于确认您选择了要擦除的正确磁盘

方法下拉列表显示了不同的可用擦除选项：

#### 擦除选项

##### 快速擦除（Quick）

仅擦除磁盘上的分区信息，使其易于重复使用而无需清除其他旧数据。 快速擦拭只需几秒钟。

##### 全零填充（Full with zeros）

用零覆盖整个磁盘，可能需要几个小时才能完成。

##### 随机填充（Full with random）

用随机二进制代码覆盖整个磁盘，完成时间比用零填充需要更长的时间。

**注意：确保备份所有数据并且不再使用磁盘。 请务必再三检查是否选择了正确的磁盘进行擦除。 从擦除的磁盘中恢复数据通常是不可能的。**

选择合适的方法后，单击擦除（**WIPE**）。 对话框要求确认操作。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageDisksWipeConfirm.png)

**验证名称以确保您选择了正确的磁盘。** 确认后可以擦除磁盘，勾选确认（**Confirm**）并单击继续（**CONTINUE**）。 会出现一个对话框显示擦除进度。

