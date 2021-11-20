# 自加密驱动器（SEDs）

[TrueNAS 版本 11.1-U5](https://www.truenas.com/docs/releasenotes/core/truenas/11.1/11.1u5/) 引入了自加密驱动器 (SED) 支持。

## 支持的规格

- 旧 ATA 设备的传统接口。 **不建议用于安全关键环境**。

- [TCG Opal 1](https://trustedcomputinggroup.org/wp-content/uploads/Opal_SSC_1.00_rev3.00-Final.pdf) 遗留规范。

- [TCG OPAL 2](https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opal_SSC_v2.01_rev1.00.pdf) 适用于较新消费级设备的标准。

- [TCG Opalite](https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opalite_SSC_FAQ.pdf) 是 OPAL 2 的简化形式。

- TCG Pyrite 版本 1 和版本 2 与 Opalite 类似，但删除了硬件加密。 Pyrite 为非 ATA 设备提供了与传统 ATA 安全性等效的逻辑。 仅驱动器固件用于保护设备。

  > Pyrite 版本 1 SEED 没有付费支持，如果密码丢失，则可能无法使用。

- [TCG Enterprise](https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-SSC_Enterprise-v1.01_r1.00.pdf) 是为有很多数据盘的系统设计的。 这些 SED 没有在操作系统启动之前解锁的功能。

有关这些规范的更多详细信息，请参阅这份 Trusted Computing Group 和 NVM Express® [联合白皮书](https://nvmexpress.org/wp-content/uploads/TCGandNVMe_Joint_White_Paper-TCG_Storage_Opal_and_NVMe_FINAL.pdf)。

## TrueNAS 实施

TrueNAS 为传统设备实现了 [camcontrol](https://www.freebsd.org/cgi/man.cgi?query=camcontrol) 的安全功能，为 TCG 设备实现了 [sedutil-cli](https://www.mankier.com/8/sedutil-cli)。从命令行管理 SED 时，建议使用 `sedutil-cli` 的 `sedhelper` 安装脚本来简化 SED 管理并解锁设备的全部功能。下面提供了使用这些命令来识别和部署 SED 的示例。

可以在将设备分配到池之前或之后配置 SED。

默认情况下，在管理员取得 SED 所有权之前不会锁定它们。通过在 Web 界面中显式配置全局或每设备密码并将密码添加到 SED 来获取所有权。在 Web 界面中添加 SED 密码还允许 TrueNAS 自动解锁 SED。

当设备从系统中物理移除时，受密码保护的 SED 会保护存储在设备上的数据。这允许安全处置设备而无需先擦除内容物。在另一个系统上重新利用 SED 需要 SED 密码。

对于 TrueNAS 高可用性 (HA) 系统，SED 驱动器仅在活动控制器上解锁。

## 部署 SED

在 **Shell** 中输入 `sedutil-cli --scan` 以检测和列出所有设备。 显示的结果的第二列标识驱动器类型：

| 字符 | 标准           |
| ---- | -------------- |
| no   | non-SED device |
| 1    | Opal V1        |
| 2    | Opal V2        |
| E    | Enterprise     |
| L    | Opalite        |
| p    | Pyrite V1      |
| P    | Pyrite V2      |
| r    | Ruby           |

例子：

```
root@truenas1:~ # sedutil-cli --scan
Scanning for Opal compliant disks
/dev/ada0  No  32GB SATA Flash Drive SFDK003L
/dev/ada1  No  32GB SATA Flash Drive SFDK003L
/dev/da0   No  HGST    HUS726020AL4210  A7J0
/dev/da1   No  HGST    HUS726020AL4210  A7J0
/dev/da10    E WDC     WUSTR1519ASS201  B925
/dev/da11    E WDC     WUSTR1519ASS201  B925
```

TrueNAS 支持为所有检测到的 SED 设置全局密码或为每个 SED 设置单独的密码。 强烈建议对所有 SED 使用全局密码以简化部署并避免为每个 SED 维护单独的密码所带来麻烦。

### 为 SED 设置全局密码

进入系统（**System**） > 高级（**Advanced**） > SED 密码（**SED Password**）并输入密码。 记录此密码并将其存放在安全的地方！

现在必须使用此密码配置 SED。 转到 `Shell` 并输入 `sedhelper setup <password>`，其中 `<password>` 是在系统（**System**） > 高级（**Advanced**） > SED 密码（**SED Password**）中输入的全局密码。

sedhelper 确保所有检测到的 SED 都正确配置为使用提供的密码：

```
root@truenas1:~ # sedhelper setup abcd1234
da9                  [OK]
da10                 [OK]
da11                 [OK]
```

每次在系统中添加/替换新 SED 时，重新运行 `sedhelper setup <password>` 以将全局密码应用于新 SED。

### 为每个 SED 创建单独的密码

转到存储（**Storage**） > 磁盘（**Disks**）。 单击已确认的 SED 的三个点图标（选项），然后单击编辑（**Edit**）。 在 SED 密码（**SED Password**）和确认 SED 密码（**Confirm SED Password fields**）字段中输入并确认密码。

存储（**Storage**） > 磁盘（**Disks**）页面会显示哪些磁盘具有配置的 SED 密码。 当磁盘有密码时，SED 密码（**SED Password**）列会显示一个标记。 非 SED 或使用全局密码解锁的磁盘不会在此列中标记。

SED 必须配置为使用新密码。 转到 **Shell** 并输入 `sedhelper setup --disk <da1> <password>`，其中 `<da1>` 是要配置的 SED，`<password>` 是从存储（**Storage**） > 磁盘（**Disks**）>编辑（**Edit**） >  SED 密码（**SED Password**） 创建的密码。

必须为每个 SED 以及将来添加到系统的任何 SED 重复此过程。

**注意：记住 SED 密码！ 如果 SED 密码丢失，则 SED 将无法解锁且其数据不可用。 每当配置或修改 SED 密码时，请始终记录下来，并将其存储在安全的地方！**

## 检查 SED 功能

在系统启动期间检测到 SED 设备时，TrueNAS 会检查配置的全局密码和特定于设备的密码。

解锁 SED 会允许存储池包含 SED 和非 SED 设备的混合。 具有独立密码的SED设备使用其密码解锁。 没有设备特定密码的设备使用全局密码解锁。

要验证 SED 锁定是否正常工作，请转到命令行管理程序。 输入 `sedutil-cli --listLockingRange 0 <password> <dev/da1>`，其中 `<dev/da1>` 是 SED，`<password>` 是该 SED 的全局或独立密码。 对于启用锁定的驱动器，该命令返回 `ReadLockEnabled: 1`、`WriteLockEnabled: 1` 和 `LockOnReset: 1`：

```
root@truenas1:~ # sedutil-cli --listLockingRange 0 abcd1234 /dev/da9
Band[0]:
    Name:            Global_Range
    CommonName:      Locking
    RangeStart:      0
    RangeLength:     0
    ReadLockEnabled: 1
    WriteLockEnabled:1
    ReadLocked:      0
    WriteLocked:     0
    LockOnReset:     1
```

## 管理 SED 密码和数据

本节包含管理 SED 密码和数据的命令行说明。 使用的命令是 [sedutil-cli(8)](https://www.mankier.com/8/sedutil-cli)。 大多数 SED 是 TCG-E（企业版）或 TCG-Opal（[Opal v2.0](https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opal_SSC_v2.01_rev1.00.pdf)）。 不同驱动器类型的命令是不同的，因此第一步是确定使用的是哪种类型。

这些命令可能会破坏数据和密码。 请注意保留备份并谨慎使用命令。
检查单个驱动器上的 SED 版本，在此示例中为 /dev/da0：

```
root@truenas:~ # sedutil-cli --isValidSED /dev/da0
/dev/da0 SED --E--- Micron_5N/A U402
```

可以一次检查所有连接的磁盘：

```
root@truenas:~ # sedutil-cli --scan
Scanning for Opal compliant disks
/dev/ada0 No 32GB SATA Flash Drive SFDK003L
/dev/ada1 No 32GB SATA Flash Drive SFDK003L
/dev/da0 E Micron_5N/A U402
/dev/da1 E Micron_5N/A U402
/dev/da12 E SEAGATE XS3840TE70014 0103
/dev/da13 E SEAGATE XS3840TE70014 0103
/dev/da14 E SEAGATE XS3840TE70014 0103
/dev/da2 E Micron_5N/A U402
/dev/da3 E Micron_5N/A U402
/dev/da4 E Micron_5N/A U402
/dev/da5 E Micron_5N/A U402
/dev/da6 E Micron_5N/A U402
/dev/da9 E Micron_5N/A U402
No more disks present ending scan
root@truenas:~ #
```

### TCG-Opal 说明

在不丢失数据的情况下重置密码：`sedutil-cli --revertNoErase <oldpassword> </dev/device>`

使用这两个命令在不破坏数据的情况下更改密码：

```
sedutil-cli --setSIDPassword <oldpassword> <newpassword> </dev/device>
sedutil-cli --setPassword <oldpassword> Admin1 <newpassword> </dev/device>
```

擦除数据并将密码重置为默认 MSID：`sedutil-cli --revertTPer <oldpassword> </dev/device>`

使用 PSID 擦除数据并重置密码： `sedutil-cli --yesIreallywanttoERASEALLmydatausingthePSID <PSINODASHED> </dev/device>` 其中 PSID 位于物理驱动器上，没有破折号 (-)。

### TCG-E 说明

#### 在不破坏数据的情况下更改或重置密码

必须为驱动器上的每个 LockingRange 或带区运行这些命令。 要确定驱动器上的频段数，请使用 `sedutil-cli -v --listLockingRanges </dev/device>`。 增加 BandMaster 编号并使用 `--setPassword` 为存在的每个频段重新运行该命令。

使用所有这些命令可以在不丢失数据的情况下重置密码：

```
sedutil-cli --setSIDPassword <oldpassword> "" </dev/device>
sedutil-cli --setPassword <oldpassword> EraseMaster "" </dev/device>
sedutil-cli --setPassword <oldpassword> BandMaster0 "" </dev/device>
sedutil-cli --setPassword <oldpassword> BandMaster1 "" </dev/device>
```

使用所有这些命令在不破坏数据的情况下更改密码：

```
sedutil-cli --setSIDPassword <oldpassword* newpassword */dev/device*
sedutil-cli --setPassword <oldpassword> EraseMaster <newpassword> </dev/device>
sedutil-cli --setPassword <oldpassword> BandMaster0 <newpassword> </dev/device>
sedutil-cli --setPassword <oldpassword> BandMaster1 <newpassword> </dev/device>
```

#### 重置密码和擦除数据

重置为默认 MSID：

```
sedutil-cli --eraseLockingRange 0 <password> </dev/device>
sedutil-cli --setSIDPassword <oldpassword> "" </dev/device>
sedutil-cli --setPassword <oldpassword> EraseMaster "" </dev/device>
```

使用 PSID 重置：

```
sedutil-cli --PSIDrevertAdminSP <PSIDNODASHS> /dev/<device>
```

如果失败，请使用：

```
sedutil-cli --PSIDrevert <PSIDNODASHS> /dev/<device>
```

