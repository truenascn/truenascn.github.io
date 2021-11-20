# 导入存储池

此过程仅适用于具有 ZFS 存储池的磁盘。 要导入具有不同文件系统的磁盘，请参阅[导入磁盘](https://www.truenas.com/docs/core/storage/importdisk/)。

ZFS 池导入适用于从当前系统导出或断开连接、在另一个系统上创建的池，以及在重新安装或升级 TrueNAS 系统后重新连接的池。 要导入池，请转至存储（**Storage** ） > 池（**Pools**） > 添加（**ADD**）。

从另一个系统物理安装 ZFS 池磁盘时，请在命令行或等效于导出该系统上的池的 Web 界面中使用 `zpool export poolname` 命令。 关闭该系统并将驱动器移至 TrueNAS 系统。 关闭原始系统可防止在 TrueNAS 导入期间出现“正在被另一台机器使用”（**“in use by another machine”**）错误。

有两种池导入，标准 ZFS 池导入和具有[传统 GELI 加密的 ZFS 池](https://docs.freebsd.org/en_US.ISO8859-1/books/handbook/disks-encrypting.html)的导入。

## 导入标准 ZFS 池

选择导入现有池（**Import Existing Pool**）并单击下一步（**NEXT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImport.png)

向导会询问池是否具有旧的 GELI 加密。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportNoGELI.png)

选择否，继续导入（**No, continue with import**）并单击下一步（**NEXT**）。

TrueNAS 检测任何存在于磁盘上但未连接的池。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportZFSPoolSummary.png)

## 导入加密的 GELI 池

**注意：导入 GELI 加密的池需要在导入之前使用加密密钥文件和密码短语来解密池。 当池无法解密时，升级失败或丢失配置后会无法重新导入，数据将无法恢复。 请始终保护好可用的池 GELI 密钥文件和密码的副本。**

选择导入现有池（**Import Existing Pool**）并单击下一步（**NEXT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImport.png)

向导会询问池是否具有旧的 GELI 加密。 选择是，解密磁盘（**Yes, decrypt the disks**）并查看解密选项。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportGELIPresentDecrypt.png)

确保“磁盘”显示作为传入池一部分的加密磁盘和分区。 通过单击选择文件并从本地系统上传文件来应用 GELI 加密密钥文件。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportGELIPresentDecryptKeyFile.png)

当密码短语也存在时，在密码短语字段中输入它。 单击下一步并等待磁盘解密。

磁盘解密后，选择要导入的 GELI 池。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportGELIPresentDecryptPool.png)

查看池导入摘要（**Pool Import Summary**）并单击导入（**IMPORT**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddImportGELIPresentDecryptPoolSummary.png)

GELI 加密池在存储（**Storage** ） > 池（**Pools**）中显示为传统加密（**Legacy Encryption**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsLegacyGELI.png)

### 备份池密钥

出于安全原因，加密池密钥不会保存到配置备份文件中。 当 TrueNAS 安装到新设备并使用保存的配置文件恢复时，加密磁盘的密钥不存在，系统不会请求它们。

要更正此问题，请在存储（**Storage**） > 池（**Pools**）中打开设置 > 导出/断开连接（**Export/Disconnect**）中导出加密池。 **不要勾选**销毁此池上的数据？（**Destroy data on this pool?**）。 现在再次导入池。 在导入过程中，如前文所述添加加密密钥。
