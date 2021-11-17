# 将 Google 云端硬盘 备份到 TrueNAS CORE

Google Drive 和 G Suite 被广泛用于创建和与团队成员共享文档、电子表格和演示文稿。 虽然基于云的工具具有云提供商包含的固有备份和复制功能，但某些用户可能需要额外的备份或存档功能。 例如，将 G Suite 用于重要工作的公司可能需要保留多年的记录，这可能超出 G Suite 订阅的范围。 TrueNAS 可以使用其内置的云同步功能轻松备份 Google Drive。

## 设置 Google 云端硬盘凭据

转至系统（**System**） > 云凭证（**Cloud Credentials**）并单击添加（**ADD**）。 命名凭据并选择 Google Drive 作为提供者。 单击登录到提供商（**LOGIN TO PROVIDER**）并使用相应的 Google 用户帐户登录。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudCredentialsAddCredentials.png)

 FreeNAS将请求Google访问设备的所有 Google Drive 文件的权限。

![](https://www.truenas.com/docs/images/TrueNASCommon/GoogleOAuthProceed.png)

![](https://www.truenas.com/docs/images/TrueNASCommon/GoogleOAuthAccount.png)

![](https://www.truenas.com/docs/images/TrueNASCommon/GoogleOAuthPermissions.png)

选择允许访问，相应的访问密钥将在 FreeNAS 访问令牌中生成。 如有必要，您可以分配团队 ID。

单击验证凭据（**VERIFY CREDENTIAL**）并等待它验证。

![](https://www.truenas.com/docs/images/TrueNASCommon/CredentialsVerify.png)

成功后，单击提交（**SUBMIT**）。 新的云凭据将在 Web 界面中可见。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudCredentials.png)

## 设置云同步任务

转到“任务”（**Tasks**）>“云同步任务”（**Cloud Sync Tasks**）并设置备份时间范围、频率和文件夹——基于云的文件夹和 TrueNAS 数据集。 设置同步是否应同步所有更改、复制新文件或移动文件。 文件从云源或 TrueNAS 源中删除，具体取决于任务是设置为推送还是拉取。 添加任务描述并选择云凭据。 选择适当的云文件夹目标和 TrueNAS 存储位置。

选择文件传输方式：

- 同步**（Sync）**：保持新创建或删除的文件相同。
- 拷贝**（Copy）**：将新文件复制到适当的目标（即，TrueNAS 从 Google Drive 提取文件或将文件推送到 Google Drive）。
- 移动**（Move）**：将文件复制到目标，然后从源中删除它们。 使用 Move，用户可以在 Google Drive 中设置一个文件夹进行存档，并将旧文档从他们的 Drive 帐户移动到该文件夹中。 然后这些文件会自动备份到它们的 TrueNAS 存储中。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudSyncCreate.png)

创建任务后，尝试运行（**Dry Run**）。

![](https://www.truenas.com/docs/images/TrueNASCommon/CloudSyncDryRun.png)

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncDryRunLog.png)

如果试运行成功，请单击“保存”以保存任务。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskNew.png)

向下展开该部分以查看任务的选项。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskNewExpanded.png)

单击现在运行（**RUN NOW**） 任务将立即开始。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncRunNow.png)

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskStarted.png)

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskRunning.png)

完成后，Web 界面将显示状态为 **RUNNING** 和 **SUCCESS**。 可以通过右上角的任务管理器（**Task Manager**）图标访问详细信息。 当任务正在运行时，单击 **RUNNING** 按钮将显示一个弹出日志。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskRunningLog.png)

一旦同步报告成功状态，您可以通过打开另一台计算机上的文件夹（如果它是共享）、通过 SSH 访问或通过 TrueNAS CLI 检查目标目录来验证它。

![](https://www.truenas.com/docs/images/CORE/12.0/CloudSyncTaskSuccess.png)

## 使用 Google 创建的内容

一个警告是，Google Docs 和其他使用 Google 工具创建的文件具有自己的专有权限集，并且它们的读/写特征是系统在标准文件共享上未知的。 结果是文件不可读。

![](https://www.truenas.com/docs/images/TrueNASCommon/GoogleDriveBadPermissions.png)

要让 Google 创建的文件变得可读，请在备份前允许链接共享访问文件。 这样做可确保其他用户可以使用读取访问权限打开文件、进行更改，然后在需要进一步编辑时另存为另一个文件。 请注意，仅当文件是使用 Google 文档、Google 表格或 Google 幻灯片创建时才需要这样做； 其他文件不应要求修改其共享设置。

![](https://www.truenas.com/docs/images/TrueNASCommon/GoogleDriveShareLink.png)

TrueNAS 非常适合长期存储内容，包括基于云的内容。 从云端同步和备份不仅简单，而且用户可以放心，他们的数据是安全的，具有快照、写入时复制和内置复制功能。

