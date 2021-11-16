# 系统更新

## 为更新和升级准备系统

系统升级或更新的准备和预防措施各不相同。 CORE更新、ENTERPRISE (HA)更新和大版本升级各有几项在您继续之前应解决的特定问题。

### 准备CORE版更新

TrueNAS CORE 具有集成更新系统，可以轻松保持最新状态。

#### 准备过程

最好在 TrueNAS 系统空闲时执行更新，没有客户端连接，没有清理或其他磁盘活动发生。大多数更新需要重新启动系统。围绕计划的维护时间规划更新以避免中断用户活动。

除非引导池中有足够的可用空间用于新的更新文件，否则更新过程不会继续。如果显示空间警告，请转至系统（**System**） > 引导（**Boot**）以删除不需要的引导备份。

#### 更新和列车（trains）

加密签名的更新文件用于更新 TrueNAS。更新文件为决定何时升级系统提供了灵活性。更新安装在新的引导备份中，让您有机会安装和测试更新，但如果出现任何问题，请在系统（**System**） > 引导（**Boot**）中恢复到以前的引导环境。

TrueNAS 定义了软件分支，称为列车（**trains**）。有几种列车可供更新，但网络界面只显示可以选择作为升级的列车。

更新列车标有数字版本以及后面跟的简短描述。当前版本接收定期错误修复和新功能。受支持的旧版 TrueNAS 仅接收维护更新。有关 TrueNAS 版本的开发和支持时间表的更多详细信息，请参阅[软件开发生命周期](https://www.truenas.com/docs/core/notices/sofdevlifecycle/)。

几个具体的词用来描述火车的类型：

稳定版（**STABLE**）：此列车提供错误修复和新功能。 STABLE 列车提供的升级经过测试，可以应用于生产环境。

**Nightlies**：用于测试 TrueNAS 未来版本的实验列车。

**SDK**：软件开发工具包培训。这有用于测试和调试 TrueNAS 的附加工具。

**注意：当当前选择的列车不适合生产使用时，用户界面会显示警告。在使用非生产列车之前，请准备好遇到错误或问题。鼓励测试人员在 https://jira.ixsystems.com 提交错误报告。**

### 准备ENTERPRISE (HA)版更新

更新为高可用性 (HA) 配置的 TrueNAS Enterprise 系统与非 HA 系统或 TrueNAS Core 的流程略有不同。 系统将更新下载到两个控制器，更新并重新启动备用 TrueNAS 控制器，最后从活动 TrueNAS 控制器进行故障转移和更新。

#### 准备过程

更新通常需要三十分钟到一个小时。 更新后需要重新启动，因此建议在维护时段内安排更新，留出两到三个小时的时间来更新、测试并在出现问题时可能回滚。 在非常大的系统上，建议按比例延长维护窗口。

如需升级期间的个别支持，请联系 iXsystems 支持以安排升级。

| 联系方式 | 联系信息                                                     |
| -------- | ------------------------------------------------------------ |
| 网站     | [https://support.ixsystems.com](https://support.ixsystems.com/) |
| 电子邮件 | [support@ixsystems.com](mailto://support.ixsystems.com)      |
| 电话     | 周一 - 周五, 上午6:00 至 下午 6:00 太平洋标准时间：1-855-473-7449 按2（仅限美国）；1-408-943-4100 按·2（国际） |
| 电话     | 7x24小时在线：1-855-499-5131（仅限美国）；1-408-878-3140（国际） |

在计划升级前至少提前两天安排时间，以确保有专家提供帮助。 从早于 9.3 版的 TrueNAS 更新必须与 iXsystems 支持一起安排。

除非引导池中有足够的可用空间用于新的更新文件，否则更新过程将不会继续。 如果显示空间警告，请转至系统（**System**） > 引导（**Boot**）并删除所有不需要的引导备份。

操作系统更新仅修改操作系统设备，不会影响存储驱动器上的最终用户数据。

更新可能涉及升级安装在存储驱动器上的 ZFS 版本。 当 ZFS 版本升级可用时，Web 界面中会出现通知警报。 不建议在存储驱动器上升级 ZFS 版本，除非已确认不需要回滚到操作系统的先前版本，并且不需要将存储驱动器与具有较早 ZFS 版本的另一个系统交换。 ZFS 版本升级后，较早的 TrueNAS 版本将无法访问存储设备。

### 准备大版本系统更新

TrueNAS 提供了使操作系统保持最新的灵活性：

1、仍然可以使用 ISO 或 Web 界面升级到主要版本，例如从 9.3 版升级到 9.10 版。除非新主要版本的发行说明表明当前版本需要 ISO 升	级，否则可以使用任一升级方法。
2、次要版本已被签名更新取代。这意味着无需等待次要版本即可使用系统更新或更新版本的驱动程序和功能来更新系统。也不再需要手	动下载升级文件及其相关校验和来更新系统。
3、更新程序会自动创建引导环境，使更新成为低风险操作。引导环境提供了通过重新引导系统并从系统（**System**） > 引导（**Boot**）菜	单中选择先前的引导	环境来返回到先前版本的操作系统的选项。

[升级说明](https://www.truenas.com/docs/core/system/update/#update-and-upgrade-instructions)描述了如何使用 .iso 文件从较早版本的 FreeNAS/TrueNAS 执行主要版本升级。有关使用 Web 界面保持系统更新的说明，请	参阅[更新文章](https://www.truenas.com/docs/core/system/update/#truenas-core)。

FreeNAS/TrueNAS 主要版本的升级路径为 9.3 > 9.10 > 11.1 > 11.3 > 12.0。始终建议升级到[受支持的软件版本](https://www.truenas.com/docs/core/notices/sofdevlifecycle/)。

#### 注意事项

在尝试进行主要版本升级之前，请注意以下注意事项：

1、**升级数据存储池可能会导致无法返回到以前的版本。**因此，更新过程不会自动升级存储池，但系统会在可以升级池时显示警报。除非	需要新的 ZFS 功能标志，否则将池保留在当前版本是安全的。如果池升级，则无法启动到不支持较新功能标志的先前 TrueNAS 版本。
2、建议将 Broadcom SAS HBA 的固件升级到最新版本。
3、从 9.3.x 升级到 9.10 时，请先阅读此[更改常见问题解答](https://www.truenas.com/docs/core/notices/9.3to9.10upgradefaq/)。
4、**不支持从 FreeNAS 0.7x 升级。**系统无法从 FreeNAS 0.7x 版本导入配置设置。必须手动重新创建配置。如果支持，必须手动导入 		FreeNAS 0.7x 池或磁盘。
5、**不支持在 32 位硬件上升级。**但是，如果系统当前运行的是 32 位版本的 FreeNAS/TrueNAS，并且硬件支持 64 位，则系统可以升级。	在升级过程中，所有存档的报告图表都将丢失。
6、**不支持 UFS。**如果数据当前驻留在一个 UFS 格式的磁盘上，请在升级后使用其他磁盘创建 ZFS 池，然后使用[导入磁盘](https://www.truenas.com/docs/core/storage/importdisk/)中的说明挂载 	UFS 格式的磁盘并将数据复制到 ZFS 池。只有一个磁盘，升级前将其数据备份到另一个系统或介质，升级后将磁盘格式化为 ZFS，然后	恢复备份。如果数据当前驻留在磁盘的 UFS RAID 上，则无法直接将该数据导入 ZFS 池。相反，升级前备份数据，升级后创建 ZFS 池，	然后从备份中恢复数据。
7、**如果您有 GELI 加密池并且要升级到 TrueNAS 12.0 或更新版本，**您可能希望将数据从 GELI 加密池迁移到 ZFS 加密池。 **GELI 池不能	转换**；必须将数据迁移到新的 ZFS 池。有关更多详细信息，请参阅加密文章。

#### 准备过程

在升级操作系统之前，请执行以下步骤：

1、在系统（**System**） > 常规（**General**） > 保存配置（**Save Config**）中备份 TrueNAS 配置。
2、备份任何可用的加密数据或拥有可用的密钥或密码。
3、警告用户在升级过程中 TrueNAS 共享数据将不可用。 建议将升级安排在对用户影响最小的时间。
4、停止所有系统服务（**Services**）。

## 更新和升级说明

### 更新TrueNAS CORE

#### 检查更新：

![](https://www.truenas.com/docs/images/CORE/12.0/SystemUpdate.png)

系统每天检查更新并下载更新（如果有）。 当有新的更新可用时会发出警报。 通过取消设置“每日检查更新”和“下载（如果可用）”来禁用更新的自动检查和下载。 单击 ⟳ (Refresh)手动执行更新检查。 要更改列车，请使用下拉菜单进行不同的选择。

列车选择器不允许降级。 例如，在引导到 Nightly 引导环境时无法选择 STABLE 列车，或者在引导到 11 引导环境时无法选择 9.10 列车。 要在测试或运行较新版本后返回较早版本，请重新启动并为较早版本选择引导环境。 然后返回这个页面检查列车的更新。

显示有关更新的信息以及指向发行说明的链接。 在更新之前阅读发行说明很重要，以确定该版本中的更改是否会影响系统的使用。

#### 保存配置文件

在安装更新之前会出现一个保存系统配置文件的对话框。

![](https://www.truenas.com/docs/images/CORE/12.0/SaveConfig.png)

**注意：保存后确保系统配置文件安全。 配置文件中的安全信息可用于未经授权访问您的 TrueNAS 系统。**

#### 应用更新

确保系统处于上述[准备更新](https://www.truenas.com/docs/core/system/update/#preparing-systems-for-updates-and-upgrades)中所述的低使用状态。 单击下载更新（**DOWNLOAD UPDATES**）按钮以立即下载并安装更新。

将出现“保存配置”对话框，以便将当前配置保存到外部媒体。

在安装更新之前会出现一个确认窗口。 设置下载后应用更新并重新启动系统时，单击继续下载并应用更新，然后重新启动系统。 您可以取消设置应用更新并在下载后重新启动系统，则只会下载更新供以后手动安装。

当更新下载并准备安装时，**APPLY PENDING UPDATE** 可见。 单击此按钮可查看确认窗口。 设置确认并单击继续安装更新并重新启动系统。

每次更新都会创建一个引导环境。 如果更新过程需要更多空间，它会尝试删除旧的引导环境。 系统（**System**） > 引导（**Boot**）中显示的标有 Keep 属性的引导环境不会被删除。 如果新引导环境的空间不可用，则升级失败。 操作系统设备上的空间可以通过转到系统（**System**） > 引导（**Boot**）并删除保留属性或删除任何不再需要的引导备份来手动释放。

TrueNAS 更新程序默认使用增量包进行更新。在更新期间，仅下载自上次更新以来在基本操作系统中发生更改的文件。 Delta 更新包通常优于完整更新包，提供更快的更新并占用更少的带宽。相比之下，完整的更新包会下载基本系统中包含的所有文件，即使这些文件没有更改。

虽然完整的软件包可能需要更多的时间来安装，但在少数情况下是必要的，例如当补丁已作为临时修复应用到本地系统时。补丁是一种用于修复主代码库中的错误的软件。虽然软件补丁通常用于修复错误，但它们也可以修复安全问题或添加新功能。

要强制进行完整更新，请打开 Web 界面 Shell 并在控制台中输入以下命令：

`freenas-update -C /tmp/update-$$ –no-delta –reboot update`

更新程序下载完整的软件包，其中包含来自最新软件版本的所有文件。下载完成后，系统以标准配置重新启动。

#### 手动更新

也可以在系统（**System**） > 更新（**Update**）中手动下载和应用更新。

手动更新不能用于大版本升级。

转到 https://download.freenas.org/ 并找到所需版本的更新文件。 手动更新文件名以 manual-update-unsigned.tar 结尾。

将所需的更新文件下载到本地系统。 登录 TrueNAS 网页界面并进入系统（**System**） > 更新（**Update**）。 单击安装手动更新文件（**INSTALL MANUAL UPDATE FILE**）。

保存配置对话框将会打开。 这使得可以将当前配置的副本保存到外部媒体，以便在出现更新问题时进行备份。

对话框关闭后，将显示手动更新屏幕。

显示当前版本的 TrueNAS 以供验证。

![](https://www.truenas.com/docs/images/CORE/12.0/UpdateManual.png)

使用浏览选择保存到本地系统的手动更新文件。 设置更新后重新启动以在安装更新后重新启动系统。 单击应用更新以开始更新。

#### 更新过程

开始更新后会显示一个进度对话框。 当更新正在进行时，Web 界面会在顶行显示一个下载/更新的动画图标。 对话框还会出现在每个活动的 Web 界面会话中，以警告正在进行系统更新。 不要中断系统更新。

### 更新TrueNAS ENTERPRISE（HA）

#### 开始更新

在 Web 界面仪表板中，找到活动 TrueNAS 控制器的条目并单击检查更新（**CHECK FOR UPDATES**）。 当有可用更新时，此按钮会更改为 **UPDATES AVAILABLE**。

![](https://www.truenas.com/docs/images/CORE/12.0/DashboardEnterprise.png)

单击该按钮转至系统（**System**） > 更新（**Update**）并显示下载更新选项，或者当系统已检测到并暂存更新时，应用之前挂起的更新。

单击“下载更新”（**Download Updates**）或“应用挂起的更新”（**Apply Pending Update**）时，首先会提供保存当前系统配置的机会。强烈建议在开始更新之前备份系统配置。在系统配置中包含密码秘密种子可从敏感系统数据（如存储的密码）中删除加密。启用此选项时，请采取额外的预防措施，将下载的系统配置文件存储在安全位置。

下载系统配置后，您可以继续下载和/或应用系统更新。这将启动更新和重新启动 TrueNAS 控制器的过程。 HA 和其他系统服务将暂时不可用。

登录到 Web 界面的其他用户将看到警告对话框。更新过程中，Web 界面的顶部栏中会显示“系统更新”图标。

显示两个 TrueNAS 控制器的更新进度。备用 TrueNAS 控制器在完成更新后会重新启动。这可能需要几分钟时间。当备用控制器完成启动后，系统必须进行故障转移以更新并重新启动活动的 TrueNAS 控制器。

#### 故障转移以完成更新

要停用活动的 TrueNAS 控制器并完成更新，请转到仪表板，找到备用控制器的条目，然后单击启动故障转移（**INITIATE FAILOVER**）。

![](https://www.truenas.com/docs/images/CORE/12.0/DashboardInitiateFailover.png)

启动故障转移会短暂中断 TrueNAS 服务和可用性。 当活动的 TrueNAS 控制器停用并且备用的 TrueNAS 控制器联机时，浏览器会从 Web 界面注销。 当备用 TrueNAS 控制器完成激活时，Web 界面登录屏幕会重新出现。

![](https://www.truenas.com/docs/images/CORE/12.0/LoginFailoverEnterprise.png)

登录 Web 界面并在顶部工具栏中检查云 HA 状态。 此图标显示 HA 在先前活动的 TrueNAS 控制器重新启动时不可用。 当 HA 可用时，会出现一个对话框，要求完成更新。 单击继续以完成更新先前活动的 TrueNAS 控制器。

通过转到仪表板并确认两个 TrueNAS 控制器上的版本相同来验证更新是否完成。

如果更新未安装在其中一个控制器上，Web 界面会生成有关控制器版本不匹配的警报。

![](https://www.truenas.com/docs/images/CORE/12.0/HAControllerVersionsErrorEnterprise.png)

如果更新出现其他问题，系统会生成警报并将详细信息写入 /data/update.failed。

您可以通过在系统引导期间激活先前的引导环境来将系统恢复到更新前的状态。 为确保版本匹配，请对两个 TrueNAS 控制器都执行此过程。 这需要对 TrueNAS 控制器控制台进行物理或 IPMI 访问。

重新启动系统并在出现启动菜单时按空格键，暂停启动过程。

![](https://www.truenas.com/docs/images/CORE/12.0/BootMenu.png)

打开 Boot Environments 菜单并循环激活引导环境，直到选择更新之前的环境。

![](https://www.truenas.com/docs/images/CORE/12.0/BootMenuSelectBE.png)

返回到第一个屏幕并按 Enter 以启动到该日期安装的 TrueNAS 版本。

企业客户应联系 iX 支持以获得更新其 TrueNAS 系统的帮助。

1、下载位于 [TrueNAS/FreeNAS 下载页面]( TrueNAS/FreeNAS 下载页面)的手动更新文件。
2、转到系统（**System**） > 更新（**Update**）。
3、单击安装手动更新（**INSTALL MANUAL UPDATE**）按钮。
4、设置 Include Password Secret Seed 复选框并单击 Save Configuration 按钮。
5、选择更新文件临时存储位置，单击选择文件按钮。 选择下载的手动升级文件。 等待文件上传，然后单击应用更新（**APPLY UPDATE**）按钮。
6、手动更新将上传文件，将文件安装到两个控制器，然后重新启动备用控制器。 要完成升级过程，请单击对话框中的关闭按钮。 按照说	明启动备用控制器的故障转移，方法是从备用控制器的仪表板卡中单击启动故障转移（**INITIATE FAILOVER**）。
7、登录系统。
8、单击 Pending Upgrade 对话框中的 Continue 按钮，备用控制器将重新启动以完成升级。

### 大版本系统更新

#### 使用ISO 升级

要使用 .iso 文件升级 TrueNAS，请访问 https://www.truenas.com/download-truenas-core/（TrueNAS CORE 最新版本）或 https://download.freenas.org 将 .iso 下载到 将用于准备安装介质的计算机。 例如，这是下载 FreeNAS 11.3 版本的 .iso 的路径：

![](https://www.truenas.com/docs/images/CORE/11.3/DownloadLatest.png)

将下载的 .iso 文件刻录到 CD 或 U 盘。 有关使用不同操作系统将 .iso 刻录到媒体的提示，请参阅安装文章中的[准备安装文件](https://www.truenas.com/docs/core/gettingstarted/install/#prepare-the-install-file)说明。

将准备好的媒体插入系统并从中启动。 在启动默认选项之前，安装程序在安装程序启动菜单中等待十秒钟。 如果需要，请按空格键停止计时器并选择另一个启动选项。 媒体完成引导进入安装菜单后，按 Enter 选择默认选项 1 安装/升级。 安装程序会显示一个显示所有可用驱动器的屏幕。

注意：显示所有驱动器，包括引导驱动器和存储驱动器。 升级时只选择引导驱动器。 **选择错误的驱动器进行升级或安装会导致数据丢失。** 如果不确定哪些驱动器包含 TrueNAS 操作系统，请重新启动并移除安装介质。 登录 TrueNAS 网页界面并进入系统（**System**） > 启动（**Boot** ） > 操作（**ACTIONS**） > 启动池状态(**Boot Pool Status**)以识别启动驱动器。 使用镜像时，会显示多个驱动器。

突出显示安装 TrueNAS 的驱动器，然后按空格键用星号标记它。 如果操作系统使用了镜像，则标记所有安装了 TrueNAS 操作系统的驱动器。 完成后按 Enter。

安装程序识别安装在引导驱动器上的早期版本的 FreeNAS/TrueNAS，并要求升级或进行全新安装：

![](https://www.truenas.com/docs/images/CORE/12.0/InstallerUpgradeChoice.png)

要执行升级，请按 Enter 接受默认的升级安装。 安装程序将显示另一个提示，提示应将操作系统安装在不用于存储的磁盘上。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallerUpgradeMethod.png)

更新后的系统可以安装在新的引导环境中，也可以格式化整个操作系统设备以重新安装。 安装到新的引导环境可以保留旧代码，如有必要，允许回滚到以前的版本。 通常不需要格式化引导设备，但可以回收空间。 在安装到新的引导环境以及格式化操作系统设备时，用户数据和设置会被保留。 将突出显示移动到选项之一，然后按 Enter 开始升级。

安装程序解压缩新映像并检查现有数据库文件的升级。 保留和迁移的数据库文件包含您的 TrueNAS 配置设置。

![](https://www.truenas.com/docs/images/CORE/12.0/InstallerUpgradePreservedDatabase.png)

按 Enter。 TrueNAS 表示升级已完成，需要重新启动。 按 OK，突出显示 3 Reboot System，然后按 Enter 重新启动系统。 如果升级安装程序是从 CD 启动的，请取出 CD。

在重新启动期间，可以将以前的配置数据库转换为新版本的数据库。 这发生在重新启动周期中的 `Applying database schema changes` 行期间。 此转换可能需要很长时间才能完成，有时十五分钟或更长时间，并且可能导致系统再次重新启动。 之后系统将正常启动。 如果显示数据库错误但 Web 界面可访问，请登录，转到系统（**System** ） > 常规（**General**），然后使用 **UPLOAD CONFIG** 按钮上传在开始升级之前下载的配置备份。
