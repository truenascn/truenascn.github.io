# TrueNAS存储配置

现在我们已登录到 Web 界面，是时候设置 TrueNAS 存储了。 这些说明演示了一个简单的镜像池设置，其中一个磁盘用于存储，另一个用于数据保护。 但是，您的存储环境有大量的配置可能性！ 您可以在深入的[池创建文章](https://www.truenas.com/docs/core/storage/pools/poolcreate/)中阅读有关这些选项的更多信息。

## 要求

系统至少需要两个大小相同的磁盘来创建镜像存储池。 虽然技术上允许使用单磁盘池，但不建议这样做。 用于 TrueNAS 安装的磁盘不计入此限制。

数据备份可以通过多种方式进行配置，并具有不同的要求。 在云中备份数据需要第 3 方云存储提供商帐户。 带复制的备份需要 TrueNAS 系统上的额外存储或（理想情况下）不同位置的另一个 TrueNAS 系统。

## 简单的存储设置

转至存储（ **Storage** ） > 池（**Pools**）并单击添加（***ADD***）。 设置创建新池并单击创建池（***CREATE POOL***）

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateManager.png)

对于名称，输入 tank 或任何其他首选名称。 在可用磁盘（**Available Disks**）中，设置两个相同的磁盘，点击移动到**Data VDevs**区域。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateTank.png)

### 添加数据集或 Zvol

新池具有“根”数据集，允许进一步划分为新数据集或 zvol（***zvols***）。 数据集（**dataset**）是存储数据并具有特定权限的文件系统。 zvol 是具有预定义存储大小的虚拟块设备。 要创建其中之一，请转至存储（**Storage**） > 池（**Pools**），单击右侧的三个点 ，然后选择添加数据集（***Add Dataset* **）或添加 Zvol（***Add Zvol***）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddDataset.png)

这些通常是作为配置特定数据共享情况的一部分创建的：

- 将数据集共享类型设置为 SMB 会针对 Windows 共享协议优化该数据集。
- 块设备共享 (iSCSI) 需要 zvol。

在将数据移入池之前，根据您的访问和数据共享要求，可以再添加其他额外的数据集或 zvol 。

完成建立和组织 TrueNAS 池后，继续配置系统[共享数据](https://www.truenas.com/docs/core/gettingstarted/sharingstorage/)的方式。

