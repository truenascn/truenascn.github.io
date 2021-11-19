# 数据集

TrueNAS 数据集是在数据存储池中创建的文件系统。 数据集可以包含文件、目录（子数据集），并具有单独的权限或标志。 也可以使用池创建的加密或单独的加密配置对数据集进行[加密](https://www.truenas.com/docs/core/storage/pools/storageencryption/)。

建议在配置数据共享之前用数据集组织池，因为这允许对访问权限进行更多微调并使用不同的共享协议。

## 创建数据集

要在所需池中创建数据集，请转到存储（**Storage**） > 池（**Pools**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePools.png)

找到该池的池和顶级（根）数据集，然后单击右侧的选项（三个小点），点击添加数据集（**Add Dataset**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddDataset.png)

要使用默认选项快速创建数据集，请输入数据集的名称并单击提交（**SUBMIT**）。

### 数据集选项

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddDatasetOptions.png)

必须配置名称（**Name**）和选项字段才能创建数据集。 数据集通常从根数据集或父数据集继承（**Inherit**）这些设置中的大部分，在单击提交（**SUBMIT**）之前只需输入数据集名称（**Name**）。

| 设置选项                          | 值                  | 描述                                                         |
| --------------------------------- | ------------------- | ------------------------------------------------------------ |
| 名称（**Name**）                  | 字符串/string       | 数据集的唯一标识符。 创建数据集后无法更改。                  |
| 备注（**Comments**）              | 字符串/stringstring | 关于数据集的注释。                                           |
| 数据同步方式（**Sync**）          | 下拉列表/drop down  | 标准（**Standard**）使用客户端软件请求的同步设置。总是（**Always**） 等待数据写入磁盘完成，而禁止（**Disabled**）从不等待写入完成，只要数据到达缓存就算完成。 |
| 压缩等级（**Compression level**） | 下拉列表/drop down  | 在比原始数据占用的空间更少的空间中编码信息。 建议选择一种在磁盘性能与节省空间量之间取得平衡的压缩算法：通常建议使用 *lz4*，因为它可以最大限度地提高性能并动态识别要压缩的最佳文件。 *zstd* 是 [Zstandard](https://tools.ietf.org/html/rfc8478) 压缩算法，它有多个选项可以平衡速度和压缩。 同样，*gzip* 选项范围从*1*（最低压缩、最佳性能）到*9*（最大压缩、最大性能影响）。 *zle* 是一种快速算法，它只消除零的运行。 *lzjb* 是不推荐使用的遗留算法。 |
| 启用访问日志（**Enable Atime**）  | 下拉列表/drop down  | *on* 在读取文件时更新文件的访问时间。 *off* 禁止在读取文件时创建日志流量以最大化性能。 |

默认情况下，数据集从根或父数据集继承加密选项。 要使用不同的加密设置配置数据集，请取消勾选继承（**Inherit**）并选择新的加密选项（**Encryption Options**）。 有关加密选项的详细说明，请参阅[加密文章](https://www.truenas.com/docs/core/storage/pools/storageencryption/)。

其他选项有助于针对特定数据共享协议调整数据集：

| 设置选项                                  | 值                 | 描述                                                         |
| ----------------------------------------- | ------------------ | ------------------------------------------------------------ |
| ZFS 重复数据删除（**ZFS Deduplication**） | 下拉列表/drop down | 透明地重复使用重复数据的单个副本以节省空间。 重复数据删除可以提高存储容量，但会占用大量内存（**RAM**），大约为 1TiB 数据去重需要 5GiB 内存。 通常建议在使用重复数据删除之前压缩数据。 重复数据删除是一种单向过程。 **数据去重一旦开启，就不能关闭！** |
| 区分大小写（**Case Sensitivity**）        | 下拉列表/drop down | 敏感（**Sensitive**）假设文件名区分大小写。 不敏感（**Insensitive**）假设文件名不区分大小写。 混合模式（**Mixed**） 会让数据集理解这两种类型的文件名。改选项 创建数据集后无法更改。 |
| 共享类型（**Share Type**）                | 下拉列表/drop down | 定义共享数据集将用于优化该共享协议的数据集的数据类型。 创建数据集后无法更改。 |

#### 高级选项

单击高级选项（**ADVANCED OPTIONS**）将数据集配额管理工具和一些附加字段添加到其他选项（**Other Options**）：

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddDatasetOptions.png)

设置配额定义了数据集的最大允许空间。 您还可以为数据集保留定义的池空间量，以帮助防止自动生成的数据（如系统日志）占用数据集上的所有空间的情况。 可以为新数据集配置配额或将所有子数据集包含在配额中。

| 设置选项                                                  | 值           | 描述                                                         |
| --------------------------------------------------------- | ------------ | ------------------------------------------------------------ |
| 数据集配额（**Quota for this datset**）                   | 整数/integer | 定义数据集的最大允许空间。 *0* 禁用配额。                    |
| 配额消耗警告百分比（**Quota warning alert at, %**）       | 整数/integer | 当消耗的空间达到定义的百分比时，生成警告级别 [alert](https://www.truenas.com/docs/core/system/alert/)。 默认情况下，数据集将从父数据集**继承**该值。 取消勾选 **Inherit** 以更改值。 |
| 配额消耗致命警告百分比（**Quota critical alert at, %**）  | 整数/integer | 当消耗的空间达到定义的百分比时，生成关键级别 [警报](https://www.truenas.com/docs/core/system/alert/)。 默认情况下，数据集将从父数据集**继承**该值。 取消勾选 **Inherit** 以更改值。 |
| 此数据集的保留空间（**Reserved space for this dataset**） | 整数/integer | 为包含日志的数据集保留额外空间，这些日志最终可能会占用所有可用空间。 *0* 是无限的。 |

更多字段添加到其他选项。 默认情况下，其中许多选项从父数据集继承（**Inherit**）它们的值。

| 设置选项                                                     | 值                 | 描述                                                         |
| ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| 只读（**Read-only**）                                        | 下拉列表/drop down | **On** 防止数据集被修改。 **Off** 允许用户访问数据集以修改其内容。 |
| 执行（**Exec**）                                             | 下拉列表/drop down | **On** 允许从该数据集中执行进程。 **Off** 阻止进程在数据集中执行。 建议设置为**On**。 |
| 显示快照目录（**Snapshot directory**）                       | 下拉列表/drop down | 控制数据集上 .zfs 目录的可见性。 在可见（**Visible**）或不可见（**Invisible**）之间进行选择。 |
| 数据副本（**Copies**）                                       | 下拉列表/drop down | 复制存储在此数据集上的 ZFS 用户数据。 在 *1*、*2* 或 *3* 冗余数据副本之间进行选择。 这可以改进数据保护和保留，但不能替代具有磁盘冗余的存储池。 |
| 记录大小（**Record Size**）                                  | 下拉列表/drop down | 数据集中的逻辑块大小。 匹配固定大小的数据（如在数据库中）可能会带来更好的性能。 |
| 启用访问控制列表模式（**ACL Mode**）                         | 下拉列表/drop down | 确定 [chmod](https://www.freebsd.org/cgi/man.cgi?query=chmod) 在调整文件 ACL 时的行为方式。 请参阅 [zfs](https://www.freebsd.org/cgi/man.cgi?query=zfs) aclmode 属性。直通模式（**Passthrough**）仅更新与文件或目录模式相关的 ACL 条目。限制模式（**Restricted**） 不允许 chmod 使用除ACL之外的方式更改文件或目录权限。 如果 ACL 可以完全表示为文件模式而不会丢失任何访问规则，则没有必要使用 ACL。 将 ACL 模式设置为限制模式（**Restricted**）通常用于优化数据集以进行 SMB 共享，但可能需要进一步优化。 例如，使用此数据集配置 [rsync 任务](https://www.truenas.com/docs/core/tasks/rsync/)可能需要在任务辅助参数字段中添加 `--no-perms`。 |
| 元数据（特殊）小块大小(**Metadata (Special) Small Block Size**) | 整数/integer       | 将小文件块包含到[特殊分配类（融合池）](https://www.truenas.com/docs/core/storage/pools/fusionpool/)中的阈值块大小。 小于或等于此值的块将分配给特殊分配类，而较大的块将分配给常规类。 有效值为 0 或 2 的幂，从 512B 到 1M。 默认大小 0 表示不会在特殊类中分配小文件块。 在设置此属性之前，必须将一个[特殊的类 vdev](https://www.truenas.com/docs/core/storage/pools/fusionpool/) 添加到池中。 |

## 管理数据集

创建数据集后，可以通过转到存储（**Storage**） > 池（**Pools**）并单击数据集右侧的三个小点来使用其他管理选项：

- 添加数据集（**Add Dataset**）: 创建一个新数据集，新数据集是该数据集的子数据集。 数据集可以以这种方式连续分层。
- 添加zvol卷（**Add Zvol**）: 创建一个新的 [ZFS 块设备](https://www.truenas.com/docs/core/storage/pools/zvols/) 作为该数据集的“子空间”。
- 修改选项（**Edit Options**）: 打开 [数据集选项](https://www.truenas.com/docs/core/storage/pools/datasets/#dataset-options) 以调整数据集配置。 数据集 名称（**Name**）、区分大小写（**Case Sensitivity**）和 共享类型（**Share Type**） 无法更改。
- 修改权限（**Edit Permissions**）: 打开编辑器以设置此数据集的访问权限。 根据数据集创建选项，这可以是简单的权限编辑器或完整的 ACL 编辑器。 有关编辑权限的更多信息，请阅读 [permissions](https://www.truenas.com/docs/core/storage/pools/permissions/) 文章。
- 用户配额（**User Quotas**）: 显示为系统上的用户帐户或连接到该系统的用户帐户设置数据或对象配额的选项。
- 用户组配额（**Group Quotas**）: 显示用于为系统上的用户组或连接到该系统的用户组设置数据或对象配额的选项。
- 删除数据集（**Delete Dataset**）: 从 TrueNAS 中删除数据集、所有存储的数据以及数据集的所有快照。

**注意：删除数据集可能会导致不可恢复的数据丢失！ 确保任何关键数据已从数据集移出或以其他方式过时。**

- 创建快照（**Create Snapshot**）: 获取数据集的单个 [ZFS 快照](https://www.truenas.com/docs/core/storage/snapshots/) 以提供额外的数据保护和移动性。 创建的快照列在存储（**Storage**）> 快照（**Snapshots**） 中。

### 配额

TrueNAS 允许为系统上或连接到系统的用户帐户和组设置数据或对象配额。

#### 用户

要查看和编辑用户配额，请转至存储（**Storage**） > 池（**Pools**）并单击以打开数据集右侧的三个小点来操作菜单。 然后单击用户配额（**User Quotas**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsUserQuotas.png)

用户配额（**User Quotas**）页面显示系统上或连接到系统的任何用户帐户的名称和配额数据。

要编辑单个用户配额，请转到用户所在行并单击右侧的三个小点，然后单击编辑图标。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsUserQuotasUserEdit.png)

编辑用户（**Edit User**）窗口允许编辑用户数据配额，即选定用户可以使用的磁盘空间量，以及用户对象配额，即每个选定用户可以拥有的文件、文件夹数量。

要批量编辑用户配额，请单击操作（**Actions**）并选择设置配额（批量）（**Set Quotas (Bulk)**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsUserQuotasBulkEdit.png)

设置配额（**Set Quotas**）窗口允许在选择任何缓存或连接的用户后编辑用户数据和对象配额。

#### 用户组

转至存储（**Storage**） > 池（**Pools**）并单击以打开数据集右侧的三个小点来操作菜单。 然后单击组配额（**Group Quotas**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsGroupQuotas.png)

组配额（**Group Quotas**）页面显示系统上或连接到系统的任何组的名称和配额数据。

要编辑单个组配额，请转到组行并单击 > 按钮，然后单击编辑图标。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsGroupQuotasEditGroup.png)

编辑组（**Edit Group**）窗口允许编辑组数据配额和组对象配额。

要批量编辑组配额，请单击操作并选择设置配额（批量）（**Set Quotas (Bulk)**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetActionsGroupQuotasBulkEdit.png)

显示了单个组的相同选项，以及为这些新配额规则选择组。
