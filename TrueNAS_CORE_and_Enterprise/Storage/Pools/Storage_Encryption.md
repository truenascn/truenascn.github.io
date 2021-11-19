# 存储加密

TrueNAS 支持关键数据的不同加密选项。

**注意：用户负责备份和保护加密密钥和密码！ 失去解密数据的能力类似于灾难性的数据丢失。**

静态数据加密可用于：

- 使用 OPAL 或 FIPS 140.2（均为 [AES 256](https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines/archived-crypto-projects/aes-development)）的[自加密驱动器 (SED)](https://www.snia.org/sites/default/education/tutorials/2009/fall/security/MichaelWillett-Self_Encrypting_Drives-FINAL.pdf)
- 特定数据集的加密（TrueNAS 12.0 中的 AES-256-GCM）

本地 TrueNAS 系统管理静态数据的密钥。 用户负责存储和保护他们的密钥。TrueNAS 12.0 中含有[密钥管理接口协议 (KMIP)](https://docs.oasis-open.org/kmip/spec/v1.1/os/kmip-spec-v1.1-os.html) 。

加密数据时，请始终考虑以下缺点/注意事项：

- 丢失加密密钥和密码意味着丢失您的数据。
- 不相关的加密数据集不支持[数据去重](https://github.com/openzfs/zfs/discussions/9423)。
- 我们不建议将 GELI 或 ZFS 加密与数据去重结合使用，因为会对性能产生巨大影响。
- 一次使用许多加密和数据去重功能时要小心，因为它们都会竞争相同的 CPU 周期

## 加密存储池

加密新存储池的根数据集可以进一步提高数据安全性。 创建一个新池并在池管理器（**Pool Manager**）中设置加密（**Encryption**）。 TrueNAS 显示警告。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddEncryptionWarning.png)

阅读警告，勾选确认（**Confirm**），然后单击我了解（**I Understand**）。

我们建议使用默认加密算法，但也可以使用其他密码。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsAddCreateEncryptionCiphers.png)

TrueNAS 支持 AES [Galois Counter Mode (GCM)](https://csrc.nist.gov/publications/detail/sp/800-38d/final) 和 [Counter with CBC-MAC (CCM)](https://tools.ietf.org/html/rfc3610) 算法进行加密。 这些算法使用分组密码提供经过验证的加密。

## 加密新数据集

TrueNAS 可以加密现有未加密存储池中的新数据集，而无需加密整个存储池。 要加密单个数据集，请转至存储（**Storage**） > 池（**Pools**），打开现有数据集右侧的三个小点，然后单击添加数据集（**Add Dataset**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetAdd.png)

在加密选项（**Encryption Options**）区域中，取消勾选继承（**Inherit**）并选中加密（**Encryption**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsCreateDatasetEncryptionOptions.png)

现在选择要使用的身份验证类型：密钥（**Key**）或密码（**Passphrase**）。 其余选项与新池相同。 启用加密的数据集在存储（**Storage**） > 池（**Pools**）列表中显示其他图标。

### 锁定和解锁数据集

数据集状态由图标确定：

- 数据集解锁图标: 一个已经解开的锁.
- 数据集锁定图标: *lock*.
- 加密池中具有与根数据集不匹配的加密属性的数据集具有此图标: ![UnecryptedPoolEncryptionDatasetIcon](https://www.truenas.com/docs/images/CORE/12.0/unecrypted_pool_encrypted_dataset.png).

注意：带有加密数据集的未加密池也将显示此图标：: ![UnecryptedPoolEncryptionDatasetIcon](https://www.truenas.com/docs/images/CORE/12.0/unecrypted_pool_encrypted_dataset.png)

加密数据集只有在使用密码而不是密钥文件进行保护时才能锁定和解锁。 在锁定数据集之前，确认它当前没有被使用，然后单击右侧的三个小点并单击锁定（**Lock**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetLockOptions.png)

仅当您确定当前没有人访问数据集时，才能使用强制卸载（**Force unmount**）选项。 锁定数据集后，解锁图标变为锁定图标。 当数据集被锁定时，它不可用。

要解锁数据集，请单击右侧的三个小点并单击解锁（**Unlock**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetUnlockOptions.png)

输入密码（**passphrase**）并单击提交（**Submit**）。 要解锁子数据集，请设置解锁子数据框。 继承父数据集加密设置的子数据集可以在父数据集解锁时解锁。 用户还可以可以通过输入他们的密码短语同时解锁具有不同密码短语的子数据集。

确认解锁数据集并等待显示解锁成功的对话框。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsDatasetUnlockSuccess.png)

#### 示例

![](https://www.truenas.com/docs/images/CORE/12.0/EncrytionExample1.png)

父数据集是`media`。 它有三个子数据集。 `documents` 子数据集“继承”了父加密设置及其密码。 另外两个子数据集（`audio` 和 `video`）有自己的密码。 当父数据集被锁定时，所有子数据集也被锁定。

![](https://www.truenas.com/docs/images/CORE/12.0/EncrytionExample2.png)

打开父数据集的选项（右侧的三个小点） 并选择解锁（**unlock**）。 要解锁所有数据集，请选中 解锁子数据集（**Unlock Children**） 并为每个需要解锁的数据集输入密码。

![](https://www.truenas.com/docs/images/CORE/12.0/EncrytionExample3.png)

单击确认解锁成功的对话窗口中的继续（**Continue**）按钮。 数据集列表更改为显示解锁图标。

## 加密管理

有两种管理加密凭证的方法：使用密钥文件（**Key Files**）或密码（**Passphrases**）：

### 密钥文件（**Key Files**）

创建新的加密池会自动生成一个新的密钥文件并提示您下载它。 **请务必将密钥文件备份到安全可靠的位置。**

![](https://www.truenas.com/docs/images/CORE/12.0/EncryptionKeyBackupWarning.png)

通过打开池设置菜单并选择导出数据集密钥（**Export Dataset Keys**），手动下载池的继承和非继承加密数据集密钥文件的副本。 输入 root 密码并单击继续（**CONTINUE**） 按钮。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEncryptionActionsExportKeys.png)

要手动下载单个数据集密钥文件的备份，请单击数据集的选项（右侧的三个小点）并选择导出密钥（**Export Key**）。 输入 root 密码并单击继续（**CONTINUE**） 按钮。 单击下载密钥（**DOWNLOAD KEY**）按钮。

要更改密钥，请单击数据集的选项（右侧的三个小点）和加密选项（**Encryption Options**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEncryptedDataset.png)

输入您的自定义密钥或单击生成密钥（**Generate Key**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StoragePoolsEncryptedDatasetOptions.png)

## 在没有密码短语的情况下解锁复制的加密数据集或 Zvol

TrueNAS Enterprise 用户在不使用密码解锁数据集或 zvol 时，可以连接密钥管理互操作性协议 (KMIP) 服务器以集中管理密钥。

使用 TrueNAS CORE 或 Enterprise 而没有安装 KMIP 的用户应该复制没有属性的数据集或 zvol 以在远程端禁用加密或构建特殊的 json 清单以使用唯一密钥解锁每个子数据集/zvol。

### 方法一：构造JSON 清单

1. 使用属性复制要复制的每个加密数据集。
2. 为每个具有唯一密钥的子数据集导出密钥。
3. 对于每个子数据集，使用目标系统的 池名/数据集名 和来自源系统的键构造一个适当的 json，如下所示： `{"tank/share01": "57112db4be777d93fa7b76138a68b790d46d6858569bf9d13e32eb9fda72146b"}`
4. 使用扩展名 .json 保存此文件。
5. 在远程系统上，使用正确构造的 json 文件解锁数据集。

### 方法二：复制没有属性的加密数据集/zvol

复制时取消选中属性，以便目标数据集不会在远程端加密并且不需要密钥来解锁。

1. 前往任务（**Tasks**） > 复制任务（**Replication Tasks**），然后单击添加（**ADD**）。
2. 单击创建高级复制 （**ADVANCED REPLICATION CREATION**）.
3. 根据需要填写表单，并确保 继承数据集属性（**Include Dataset Properties **)未被选中。
4. 单击提交（**SUBMIT**）。

注意：这不会影响使用 KMIP 安装 TrueNAS Enterprise。

## 传统 GELI 加密

TrueNAS 不再支持 GELI 加密（已弃用）。

**Q**：我可以将 GELI 加密的池直接转换为原生 ZFS 加密吗？

**A**：不可以。您必须将数据从 GELI 池迁移到 ZFS 加密池。

### GELI 池迁移

数据可以从 GELI 加密池迁移到新的 ZFS 加密池。 在尝试任何数据迁移之前，请务必解锁 GELI 加密池。 新的 ZFS 加密池的大小必须至少与之前的 GELI 加密池的大小相同。 在验证数据迁移之前不要删除 GELI 数据集。

有几个选项可以将数据从 GELI 加密池迁移到新的 ZFS 加密池：

#### 复制向导（Replication Wizard）

在 TrueNAS Web 界面中继续检测和支持 GELI 加密池作为“传统加密”池。 从 TrueNAS 版本 12.0-U1 开始，解密的 GELI 池可以使用复制向导将数据迁移到新的 ZFS 加密池。

通过任务（**Tasks**） > 复制任务（**Replication Tasks**），然后单击添加（**ADD**）， 启动复制向导

##### 源位置（Source Location）

- 选择在此系统上（**On this System**）。
- 设置要传输的数据集。

##### 目的位置（Destination Location）

- 选择在不同的系统上（**On a Different System**）。

##### SSH连接（SSH Connection）

- 通过单击新建（**Create New**）或选择目标系统的 ssh 连接来创建 ssh 连接。
- 在目标（**Destination**）中，选择要将文件复制到的数据集。
- 设置加密（**Encryption**）。
- 为加密密钥格式（**Encryption Key Format**）选择 PASSPHRASE 或 HEX。
- 如果您选择了密码，请输入密码。 如果您选择了 HEX，请设置生成加密密钥（**Generate Encryption Key**）。
- 设置在发送 TrueNAS 数据库中存储加密密钥（***Store Encryption key in Sending TrueNAS database***）。
- 点击下一步

##### 复制日程计划（Replication Schedule）

- 在复制计划中设置运行一次（**Run Once**）。
- 取消勾选目标数据集只读（**Make Destination Dataset Read-Only**）。
- 点击开始复制（**START REPLICATION**）。

#### 直接文件传输（File Transfer）

主要：此方法不保留文件 ACL。

Web 界面支持使用任务（**Tasks**） > 云同步任务（**Rsync Tasks**） 将文件传输出 GELI 池。 在 Shell 中，rsync 和其他文件传输机制（scp、cp、sftp、ftp、rdiff-backup）可用于在池之间复制数据。

#### ZFS 发送和接收（ZFS Send and Receive）

这些说明是示例。 它不是适用于所有情况的精确分步指南。 在尝试之前请研究 ZFS 发送/接收。 一个简单的例子不能涵盖所有边缘情况。

```
GELI Pool = pool_a
Origin Dataset = dataset_1
Latest Snapshot of GELI Pool = snapshot_name
ZFS Native Encrypted Pool = pool_b
Receieving Dataset = dataset_2
```

1. 在存储（**Storage**） > 存储池（**Pools**）中创建一个新的加密池.
2. 打开**Shell**. 使用要迁移的数据制作 GELI 池和数据集的新快照： `zfs snapshot -r pool_a/dataset_1@snapshot_name`.
3. 创建密码： `echo passphrase > /tmp/pass`.
4. 使用 ZFS 发送/接收在池之间传输数据： `zfs send -Rv pool_a/dataset_1@snapshot_name | zfs recv -o encryption=on -o keyformat=passphrase -o keylocation=file:///tmp/pass pool_b/dataset_2`.
5. 传输完成后，转到存储（**Storage**） > 存储池（**Pools**）并锁定新数据集。锁定数据集后，立即解锁。 TrueNAS 提示输入密码。输入密码并解锁池后，您可以删除用于传输的 `/tmp/pass` 文件。
6. 如果需要，您可以转换数据集以使用密钥文件而不是密码。要使用密钥文件，请单击数据集右侧的三个小点（选项）然后单击加密选项（**Encryption Options**），将加密类型（ **Encryption Type**）从密码短语（**Passphrase**）更改为密钥（**Key**）并保存。立即备份您的密钥文件！
7. 对池中需要迁移的每个数据集重复此过程。

