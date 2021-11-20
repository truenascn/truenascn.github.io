# 导入磁盘

使用存储（**Storage**） > 导入磁盘（**Import Disk**）将 UFS (BSD Unix)、NTFS (Windows)、MSDOS (FAT) 或 EXT2 (Linux) 格式的磁盘中的数据导入到 TrueNAS 中。 这是一次性导入，将数据从该磁盘复制到 TrueNAS 数据集。 一次只能导入一张磁盘，并且该磁盘必须安装或物理连接到 TrueNAS 系统。

**Q**：EXT3 或 EXT4 文件系统怎么导入？

**A**：在某些情况下可以导入 EXT3 或 EXT4 文件系统，尽管两者都不受完全支持。 不支持 EXT3 日志，因此这些文件系统必须有一个外部 `fsck` 实用程序，如 [E2fsprogs 实用程序](http://e2fsprogs.sourceforge.net/)，在导入之前在它们上运行。 不支持扩展属性或 inode 大于 128 字节的 EXT4 文件系统。 如上所述，具有 EXT3 日志的 EXT4 文件系统在导入之前必须在其上运行 `fsck`。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageImportDisk.png)

使用下拉菜单选择要导入的磁盘。

TrueNAS 尝试检测并选择文件系统类型（**Filesystem type**）。 选择 MSDOSFS 文件系统会显示一个附加的 MSDOSFS 区域（**MSDOSFS locale**）设置下拉菜单。 当磁盘上存在非 ASCII 字符时，需要使用此选项选择区域设置。

最后，浏览到 ZFS 数据集以将复制的数据保存到目标路径（**Destination Path**）。

单击保存（**SAVE**）后，所选磁盘将挂载并将其内容复制到目标路径（**Destination Path**）末尾的指定数据集。 要监视正在进行的导入，请通过单击界面顶部栏中的日志图标打开任务管理器。 复制操作完成后，磁盘将卸载。 系统会显示一个对话框以供用户查看或下载磁盘导入日志。

**Q**：导入中断！我该如何处理？

**A**：使用相同的导入过程重新启动任务。 选择与 TrueNAS 中断导入相同的目标路径以扫描先前导入文件的目标并继续导入其它的剩余文件。

