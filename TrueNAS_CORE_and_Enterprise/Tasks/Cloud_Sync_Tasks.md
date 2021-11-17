# 云同步任务

TrueNAS 可以发送、接收或与云存储提供商同步数据。 云同步任务允许按计划进行单次传输或重复传输，是将数据备份到远程位置的有效方法。

**注意：使用云意味着数据可以转到与 iXsystems 没有直接关联的第三方商业供应商。 在创建任何 Cloud Sync 任务之前，请调查并充分了解供应商的定价政策和服务。 iXsystems 不对因使用具有云同步功能的第三方供应商而产生的任何费用负责。**

TrueNAS 支持 Amazon S3、Google Cloud 和 Microsoft Azure 等主要供应商以及各种其他供应商。 要查看受支持供应商的完整列表，请转至系统（**System**） > 云凭证（**Cloud Credentials**） > 添加（**Add**）并打开供应商下拉菜单。

## 要求

- 所有系统 [存储](https://www.truenas.com/docs/core/storage/) 都必须配置并准备好接收或发送数据。
- 云存储提供商提供的帐户和云存储位置必须可用。
- 在创建同步任务之前，必须将 Cloud Storage 帐户凭据保存到 系统（**System**） > 云凭证（**Cloud Credentials**）。 具体说明见[云凭证](https://www.truenas.com/docs/core/system/cloudcredentials/)。

## 创建云同步任务

转至任务（**Tasks**） > 云同步任务（**Cloud Sync Tasks**），然后单击添加。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudSyncAdd.png)

为任务提供描述，并选择现有的云凭据。 TrueNAS 会连接到所选的云存储提供商并显示可用的存储位置。 确定数据是从本系统传输到 (PUSH) 还是从云存储位置（**Remote**）传输到本系统 (PULL) 。 接下来选择传输模式：

### 传输模式

#### 同步（Sync）

同步使两个存储位置之间的所有文件保持相同。 如果同步遇到错误，则不会删除目标中的文件。 这包括当 [Dropbox 版权检测器](https://techcrunch.com/2014/03/30/how-dropbox-knows-when-youre-sharing-copyrighted-stuff-without-actually-looking-at-your-stuff/)将文件标记为受版权保护时的常见错误。

请注意，同步到 Backblaze B2 存储桶不会从存储桶中删除文件，即使这些文件已在本地删除。 相反，文件会用版本号标记或移动到隐藏状态。 要从存储桶中自动删除旧的或不需要的文件，请调整 [Backblaze B2 保存时间规则](https://www.backblaze.com/blog/backblaze-b2-lifecycle-rules/)。

无法通过同步删除存储在 Amazon S3 Glacier 或 S3 Glacier Deep Archive 中的文件。 这些文件必须首先通过其他方式恢复，例如 [Amazon S3 控制台](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/restore-archived-objects.html)。

#### 拷贝（Copy）

拷贝将每个源文件复制到目标中，覆盖目标中与源同名的所有文件。 复制是潜在破坏性最小的选择。

#### 移动（Move）

移动将文件从源传输到目标，并删除原始源文件。同时目标上具有相同名称的文件将被覆盖。

接下来，通过定义计划来控制任务何时运行。 当需要特定计划时，选择自定义并使用高级计划程序。

### 高级计划程序

![](https://www.truenas.com/docs/images/CORE/12.0/TasksAdvancedScheduler.png)

选择预设会填充其余字段。要自定义计划，请按照`Minutes/Hours/Days`输入 [crontab](https://www.freebsd.org/cgi/man.cgi?crontab%285%29) 值。

请填写标准的 cron 值。最简单的选择是在字段中输入一个数字。当时间值与该数字匹配时，任务将运行。例如，输入 10 表示任务在时间过十分钟时运行。

星号 (*) 表示“匹配所有值”。

通过输入带连字符的数字值来设置特定的时间范围。例如，在“分钟”（**Minutes**）字段中输入 30-35 会将任务设置为在 30、31、32、33、34 和 35 分钟运行。

也可以输入值列表。输入由逗号 (,) 分隔的各个值。例如，在“小时”（**Hours**）字段中输入 1,14 表示任务在凌晨 1:00 (0100) 和下午 2:00 (1400) 运行。

斜线 (/) 表示步长值。例如，在 "日期"（**Days**） 中输入 * 表示该任务在每月的每一天运行，*/2 表示该任务每隔一天运行一次。

将上述所有示例结合在一起，可以创建一个计划，在每天凌晨 1:30-1:35 和下午 2:30-2:35 每分钟运行一次任务。

有一个选项可以选择任务运行的月份。不设置每个月与选择每个月相同。

星期几安排任务在特定的日子运行。这是对任何列出的天数的补充。例如，在日期中输入 1 并将 **Wed** 设置为 **Days of Week** 会创建一个计划，该计划在每月的第一天和每月的每个星期三启动任务。

计划预览（**Schedule Preview**）显示当前设置的任务运行的时间。

#### CRON 语法示例

| 句法                | 含义                                                       | 例子                                                         |
| ------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| *                   | 每一项。                                                   | * (minutes) = 每小时的每个特定分钟. * (days) = 每天.         |
| */N                 | 每 N 个项目                                                | */15 (minutes) = 每小时的第 15 分钟（每刻钟）。 */3 (days) = 每 3 天。 */3 (months) = 每三个月一次。 |
| 逗号和连字符/破折号 | 每个陈述的项目（逗号） 范围内的每个项目（连字符/破折号）。 | 1,31 (minutes) = 在每小时的第 1 分钟和第 31 分钟。 1-3,31 (minutes) = 每小时的第 1 分钟到第 3 分钟，以及第 31 分钟。 mon-fri (days) = 每周一至周五（包括每个工作日）。 mar,jun,sep,dec (months) = 每年三月、六月、九月、十二月。 |

天数可以指定为一个月的天数或一周的天数。

使用这些选项，可以创建类似于以下示例的灵活计划：

| 想要的时间表                                                 | 要输入的值                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 每天 3 次（午夜、08:00 和 16:00）                            | months=*; days=*; hours=0/8 or 0,8,16; minutes=0 (Meaning: every day of every month, when hours=0/8/16 and minutes=0) |
| 每周一、周三和周五，晚上 8.30                                | months=*; days=mon,wed,fri; hours=20; minutes=30             |
| 每月的第 1 天和第 15 天，10 月至 6 月，上午 00:01            | months=oct-dec,jan-jun; days=1,15; hours=0; minutes=1        |
| 工作日内每 15 分钟一次，即周一至周五上午 8 点至晚上 7 点（08:00 - 19:00） | Note that this requires two tasks to achieve: (1) months=*; days=mon-fri; hours=8-18; minutes=*/15 (2) months=*; days=mon-fri; hours=19; minutes=0 We need the second scheduled item, to execute at 19:00, otherwise we would stop at 18:45. Another workaround would be to stop at 18:45 or 19:45 rather than 19:00. |

取消勾选启用（**Enable**）使配置可用，而不定期运行任务。 要手动激活已保存的任务，请转至任务（**Tasks**） > 云同步任务（**Cloud Sync Tasks**），单击 > 展开任务，然后单击立即运行（**RUN NOW**）。

其余选项允许根据您的特定要求调整任务。

### 选项

#### 传输

| 项目名称                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| 描述（**Description**）          | 输入云同步任务的描述。                                       |
| 方向（**Direction**）            | PUSH 将数据发送到云存储。 PULL 从云存储接收数据。 更改方向会将传输模式重置为复制。 |
| 传输模式（**Transfer Mode**）    | 同步：目标上的文件被更改以匹配源上的文件。 如果源上不存在文件，它也会从目标中删除。 COPY：来自源的文件被复制到目标。 如果目标上存在具有相同名称的文件，它们将被覆盖。 MOVE：文件从源复制到目标后，从源中删除。 目标上具有相同名称的文件将被覆盖。 |
| 目录/文件（**Directory/Files**） | 选择要发送到云以进行推送同步的目录或文件，或选择要写入拉取文件的目的地。 请注意拉取作业的目的地，以免覆盖现有文件。 |

#### 远程

| 项目名称               | 描述                                     |
| ---------------------- | ---------------------------------------- |
| 凭据（**Credential**） | 从可用云凭据列表中选择云存储提供商凭据。 |

#### 控制

| 项目名称             | 描述                                                    |
| -------------------- | ------------------------------------------------------- |
| 日程（**Schedule**） | 选择计划预设或选择自定义以打开高级计划程序。            |
| 启用（**Enabled**）  | 启用此云同步任务。 取消勾选禁用此云同步任务而不删除它。 |

#### 高级选项

| 项目名称                            | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| 按照符号链接（**Follow Symlinks**） | 按照符号链接并复制它们链接到的项目。                         |
| 预执行脚本（**Pre-Script**）        | 在运行同步之前执行的脚本。                                   |
| 后执行（**Post-Script**）           | 运行同步后要执行的脚本。                                     |
| 排除（**Exclude**）                 | 要从同步中排除的文件和目录列表。 按 Enter 分隔条目。 用于排除文件/目录的正确语法示例如下：`photos` 将排除名为“photos”的文件；`/photos` 将从根目录（但不是子目录）中排除名为“photos”的文件；`photos/` 将排除名为“photos”的文件夹；`/photos/` 将从根目录（但不包括子目录）中排除名为“photos”的目录。有关 `--exclude` 选项的更多详细信息，请参阅 [rclone 过滤](https://rclone.org/filtering/)。 |

#### 高级远程选项

| 项目名称                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| 远程加密（**Remote Encryption**） | *PUSH*：传输前加密文件并将加密文件存储在远程系统上。 使用加密密码和加密密钥对文件进行加密。 *PULL*：在传输之前解密存储在远程系统上的文件。 传输加密文件需要输入用于加密文件的相同加密密码和加密密钥。 有关加密算法和密钥派生的其他详细信息，请参阅 [rclone crypt 文件格式文档](https://rclone.org/crypt/#file-formats)。 |
| 传输线程（**Transfers**）         | 同时传输的文件数。 根据可用带宽和目标系统性能输入一个数字。 请参阅 [rclone –transfers](https://rclone.org/docs/#transfers-n)。 |
| 流量限制（**Bandwidth limit**）   | rclone 格式的单个带宽限制或带宽限制计划。 按 Enter 分隔条目。 示例：`08:00,512 12:00,10MB 13:00,512 18:00,30MB 23:00,off`。 单位可以用开头字母指定：b、k（默认）、M 或 G。请参阅 [rclone –bwlimit](https://rclone.org/docs/#bwlimit-bandwidth-spec)。 |

#### 脚本和环境变量

高级用户可以编写在 Cloud Sync 任务之前或之后立的脚本。 **Post-script** 字段仅在 Cloud Sync 任务成功完成时运行。 您可以将各种任务环境变量传递到 **Pre-script** 和 **Post-script** 字段中：

- CLOUD_SYNC_ID
- CLOUD_SYNC_DESCRIPTION
- CLOUD_SYNC_DIRECTION
- CLOUD_SYNC_TRANSFER_MODE
- CLOUD_SYNC_ENCRYPTION
- CLOUD_SYNC_FILENAME_ENCRYPTION
- CLOUD_SYNC_ENCRYPTION_PASSWORD
- CLOUD_SYNC_ENCRYPTION_SALT
- CLOUD_SYNC_SNAPSHOT

还有提供者特定的变量，如 CLOUD_SYNC_CLIENT_ID 或 CLOUD_SYNC_TOKEN 或 CLOUD_SYNC_CHUNK_SIZE

远程存储设置：

- CLOUD_SYNC_BUCKET
- CLOUD_SYNC_FOLDER

本地存储设置：

- CLOUD_SYNC_PATH

### 测试设置

在单击 **DRY RUN** 保存之前测试设置。 TrueNAS 连接到云存储提供商并模拟文件传输。 这个过程没有实际发送或接收数据。会出现 一个对话框显示测试状态并允许下载任务日志。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudsyncAddGoogledriveDryrun.png)

## 运行云同步

已保存的任务将根据其计划或通过单击“立即运行”来激活。 正在进行的云同步必须先完成，然后才能开始。 停止正在进行的任务会取消文件传输并需要重新开始文件传输。

要查看有关正在运行或最近运行的任务的日志，请单击任务状态。

## 云同步还原

要快速创建使用相同选项但反转数据传输方向的新云同步，请展开现有云同步的 > 并单击恢复（**RESTORE**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksCloudSyncRestore.png)

为这个反向任务输入一个新的描述，并定义传输数据的存储位置的路径。

还原的云同步任务会被保存为任务（**Tasks**） > 云同步任务（ **Cloud Sync Tasks**）中的另一个条目。

如果还原目标数据集与原始源数据集相同，则还原文件的所有权可能更改为 root。 如果原始文件不是由 root 创建的，并且需要不同的所有者，您可以通过 GUI 或从 CLI 运行 chown 递归重置恢复数据集的 ACL 权限。

