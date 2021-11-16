# TrueNAS数据备份

创建和共享存储后，是时候确保有效备份 TrueNAS 数据了。 TrueNAS 提供了多种备份数据的选项。

## 一、云同步

云同步需要云存储提供商的帐户和由提供商创建的存储位置，例如 Amazon S3 存储桶。 支持 Amazon S3、Google Cloud、Box 和 Microsoft Azure 等主要供应商以及各种其他供应商。 这些可能会收取数据传输和存储费用，因此请在传输任何数据之前查看您的云存储提供商的政策。

您可以将 TrueNAS 配置为发送、接收或与云存储提供商同步数据。 配置 Cloud Sync 任务允许您一次性传输数据或设置重复计划以定期传输数据。

### 添加凭据

转至系统（**System**） > 云凭证（**Cloud Credentials**） > 添加（**ADD**）。 输入名称（***Name***）并从下拉菜单中选择提供商（***Provider***）。 身份验证选项会根据所选的提供程序而变化。 凭证要么必须手动输入，要么需要一个提供商登录并自动添加凭证。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataCloudSyncAuth.png)

输入提供者凭据后，单击验证凭据（***VERIFY CREDENTIAL***）。 确认验证后，单击提交（***SUBMIT***）。

### 添加数据传输任务

转至任务（**Tasks **） > 云同步任务（**Cloud Sync Tasks**），然后单击添加（***ADD***）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudSyncAdd.png)

选择先前保存的凭据以填充远程（**Remote**）部分。

为任务添加描述，选择 PUSH 或 PULL 作为方向，选择 COPY 作为传输模式。 在目录/文件下，选择先前创建的数据集（此处为**tank**）。

现在，使用控制（**Control**）选项来定义此任务的运行频率。 当运行任务对您的网络干扰最小时，打开计划下拉菜单并选择预设时间。 当任务只需要运行一次时，取消设置已启用（***Enabled***）。 然后可以从“任务“（**Tasks**）>“云同步任务”（**Cloud Sync Tasks** ）列表中一次性触发该任务以执行初始迁移或备份。

要测试您的任务，请单击 DRY RUN。 试运行成功后，单击提交保存任务并将其添加到“任务“（**Tasks**）>“云同步任务”（**Cloud Sync Tasks** ）列表。

要手动运行任务，请转至“任务“（**Tasks**）>“云同步任务”（**Cloud Sync Tasks** ）列表，单击 ”>“ 展开新任务，然后单击立即运行（***RUN NOW***）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudSyncOptions.png)

状态（**Status**）会显示成功或失败。 单击状态条目可查看操作的详细日志。

## 二、复制

复制是获取数据的时间“快照”并将该快照复制到另一个位置的过程。 快照通常比完整文件备份使用更少的存储空间并且有更多的管理选项。 此说明显示使用 TrueNAS 向导创建简单复制。

转至任务（**Tasks**） > 复制任务（**Replication Tasks**）并单击添加（***ADD***）。 将源位置设置为本地系统并选择要快照的数据集。 当找不到现有的源快照时，向导会为源创建新的快照。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataRepTaskSource.png)

将目标设置为本地系统并定义复制快照的存储位置的路径。 手动定义目的地时，请务必键入目的地位置的完整路径。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataRepTaskDestination.png)

您可以为此复制定义一个特定的计划，也可以选择在保存新任务后立即运行它。 未计划的任务仍保存在复制任务列表中，可以手动运行或稍后编辑以添加计划。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataRepTaskSchedule.png)

单击 **START REPLICATION** 保存新任务并立即尝试将快照复制到目标。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataRepTaskCompletion.png)

要确认快照已被复制，请转到存储（**Storage**） > 快照（**Snapshots**）并验证目标数据集是否具有带有正确时间戳的新快照。

![](https://www.truenas.com/docs/images/CORE/12.0/StoringDataRepTaskVerified.png)

TrueNAS 现在可以访问并配置为存储、共享和备份您的数据！

如果您需要扩展系统功能，请参阅有关其他[应用程序](https://www.truenas.com/docs/core/gettingstarted/applications/)的其余文章。 当您准备好微调系统配置或了解有关高级功能的更多信息时，请参阅 TrueNAS CORE 和 Enterprise 部分中的其余部分。 这些部分按照在 TrueNAS 界面中出现的顺序进行组织，以及有关[第三代方解决方案](https://www.truenas.com/docs/core/solutions/)的附加主题、[API 参考指南](https://www.truenas.com/docs/core/api/)、来自 iXsystems, Inc 的[官方通知](https://www.truenas.com/docs/core/notices/)和[社区建议](https://www.truenas.com/docs/core/userrecommends/)。

