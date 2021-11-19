# ZFS 卷 (Zvol)

ZFS 卷 (Zvol) 是表示块设备的数据集。 配置 iSCSI 共享时需要这些。

要在池中创建 zvol，请转到存储（**Storage**） > 池（**Pools**），然后单击添加 Zvol（**Add Zvol**）。

选项

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsCreateZvol.png)

要使用默认选项快速创建 Zvol，请输入 Zvol 的名称、大小，然后单击“保存”（**SAVE**）。

| 设置选项                                     | 值                      | 描述                                                         |
| -------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| 卷名称（**Zvol name**）                      | 字符串/string           | 输入 zvol 的名称。 使用超过 63 个字符的 zvol 名称会阻止将 zvol 作为设备访问。 例如，具有 70 个字符的文件名或路径的 zvol 不能用作 iSCSI 扩展。 此设置是强制性的。 |
| 备注（**Comments**）                         | 字符串/string           | 输入有关此 zvol 的注释。                                     |
| Zvol卷大小（**Size for this zvol**）         | 整数/integer            | 指定大小和值。 可以使用“t”、“TiB”和“G”等单位。 zvol 的大小可以在以后增加，但不能减少。 如果大小超过存储池可用容量的 80%，除非同时启用“强制大小”，否则创建将失败并显示“空间不足”错误。 |
| 强制创建（**Force size**）                   | 复选框/checkbox         | 默认情况下，如果创建zvol将使池容量占用超过 80%，系统将不会创建 zvol。 **虽然不推荐**，但启用此选项将强制创建 zvol。 |
| 同步（**Sync**）                             | 下拉菜单/drop-down menu | 标准（**Standard**）使用客户端软件请求的同步设置。总是（**Always**） 等待数据写入磁盘完成，而禁止（**Disabled**）从不等待写入完成，只要数据到达缓存就算完成。 |
| 压缩等级（**Compression level**）            | 下拉菜单/drop-down menu | 压缩数据以节省空间。 有关可用算法的说明，请参阅压缩。        |
| ZFS数据去重（**ZFS Deduplication**）         | 下拉菜单/drop-down menu | 除非您的 iXsystems 支持工程师指示这样做，否则不要更改此设置。 |
| 简易模式（**Sparse**）                       | 复选框/checkbox         | 用于提供精简配置。 谨慎使用，因为当池空间不足时写入将失败。  |
| 只读（**Read-only**）                        | 下拉菜单/drop-down menu | 设置为防止 zvol 被修改。                                     |
| 继承加密（**Inherit (Encryption Options)**） | 复选框/checkbox         | 启用会导致 zvol 使用根数据集的加密属性。                     |

高级选项

| 设置选项                   | 值                      | 描述                                                         |
| -------------------------- | ----------------------- | ------------------------------------------------------------ |
| 区块大小（**Block size**） | 下拉菜单/drop-down menu | 默认为继承（**Inherit**），其他选项包括*4KiB*、*8KiB*、*16KiB*、*32KiB*、*64KiB*、*128KiB* |

最佳 Zvol 块大小：

TrueNAS 会自动为新 zvol 推荐节省空间的块大小。 此表显示了推荐的最小卷块大小值。 要手动更改此值，请使用块大小下拉菜单。

| 配置模式 | 驱动器数目 | 最佳区块大小 |
| -------- | ---------- | ------------ |
| Mirror   | N/A        | 16k          |
| Raidz-1  | 3          | 16k          |
| Raidz-1  | 4/5        | 32k          |
| Raidz-1  | 6/7/8/9    | 64k          |
| Raidz-1  | 10+        | 128k         |
| Raidz-2  | 4          | 16k          |
| Raidz-2  | 5/6        | 32k          |
| Raidz-2  | 7/8/9/10   | 64k          |
| Raidz-2  | 11+        | 128k         |
| Raidz-3  | 5          | 16k          |
| Raidz-3  | 6/7        | 32k          |
| Raidz-3  | 8/9/10/11  | 64k          |
| Raidz-3  | 12+        | 128k         |

根据工作负载，可能需要额外调整以获得最佳性能。 iXsystems 工程师可以帮助企业客户调整他们的 TrueNAS 硬件。也可参考OpenZFS 手册的[工作负载调优章节](https://openzfs.github.io/openzfs-docs/Performance%20and%20Tuning/Workload%20Tuning.html)。

## 管理 Zvol

要查看现有 zvol 的选项，请在存储（**Storage**） > 池（**Pools**）中单击所需 zvol 旁边的三个小点：

- 删除zvol（**Delete zvol**）：从 TrueNAS 中删除 zvol。

  **注意：删除 zvol 会导致不可恢复的数据丢失！ 确保任何关键数据已从 zvol 移出或以其他方式过时。**

  删除 zvol 也会删除该 zvol 的所有快照。

- 编辑zvol（**Edit Zvol**）： 打开 zvol 创建表单以更改先前保存的设置。 与数据集类似，zvol 名称不能更改。

- 创建快照（**Create Snapshot**）： - 获取 zvol 的单个当前时间点镜像并将其保存到存储（**Storage**） > 快照（**Snapshots**）。 建议使用快照专属名称（**Name**），并且可以使用额外的选项使快照递归（**Recursive**） 可用。

从现有[快照](https://www.truenas.com/docs/core/storage/snapshots/)克隆选定的 zvol 时，广义数据集（**Promoting Dataset**） 可用。 克隆时，原始卷将成为该克隆的克隆，从而可以删除创建该快照的卷。 否则，当原始卷存在时，无法删除快照。

在启用[加密](https://www.truenas.com/docs/core/storage/pools/storageencryption/)的情况下创建 zvol 时，会显示其他加密操作（**Encryption Actions**）。

