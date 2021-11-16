# 硬盘S.M.A.R.T.检测

S.M.A.R.T.（自我监控、分析和报告技术）是磁盘监控和测试的行业标准。 可以使用几种不同类型的自检来监控磁盘的问题。 TrueNAS 可以调整 S.M.A.R.T. 警报的时间和方式。 当 S.M.A.R.T. 监控报告问题，我们会建议您更换该磁盘。 大多数现代 ATA、IDE 和 SCSI-3 硬盘驱动器都支持 S.M.A.R.T. 请参阅您各自的驱动器文档进行确认。

S.M.A.R.T. 测试在磁盘上运行。 运行测试会降低驱动器性能，因此我们建议在系统处于低使用状态时安排测试。 避免同时安排磁盘密集型测试！ 例如，S.M.A.R.T. 测试不应与磁盘[清理](https://www.truenas.com/docs/core/tasks/scrubtasks/)或[重新同步](https://www.truenas.com/docs/core/tasks/resilverpriority/)安排在同一天。

转至存储（**Storage**） > 磁盘（**Disks**），然后单击 > 以展开条目。 启用 S.M.A.R.T. （**Enable S.M.A.R.T.**）显示为是（**true**）或否（**false**）。

要启用或禁用测试，请单击 **EDIT DISK(S)** 并找到  启用 S.M.A.R.T. （**Enable S.M.A.R.T.**） 选项。

## 手动S.M.A.R.T.检测

要快速测试磁盘是否有错误，请转到存储（**Storage**） > 磁盘（**Disks**）并选择要测试的磁盘。 选择所需的磁盘后，单击手动测试（**MANUAL TEST**）。

![](https://www.truenas.com/docs/images/CORE/12.0/StorageDisksManualTestOptions.png)

接下来，选择测试类型。 根据驱动器连接、ATA 或 SCSI，测试类型可能会略有不同：

### ATA

1、长期（**Long**） - 运行 SMART 扩展自检。 这将扫描整个磁盘表面，在大容量磁盘上可能需要数小时。
2、短期（**Short**） - 运行 SMART 短期自检（通常不到 10 分钟）。 这些是基本磁盘测试，因制造商而异。
3、Conveyance - 运行 SMART Conveyance 自检。 此自检程序旨在识别设备运输过程中发生的损坏。 此自检程序仅需几分钟即可完成。
4、离线（**Offline**） - 运行 SMART 立即离线测试。 此测试的效果仅在它更新 SMART 属性值时可见，如果测试发现错误，它们会出现在 SMART 错误日志中。

### SCSI

1、长期（**Long**） - 运行“后台长”自检。
2、短期（**Short**）- 运行“Background short”自检。
3、离线（**Offline** - 在前台运行默认的自检。 自检日志中未放置任何条目。

有关更多信息，请参阅 [smartctl(8)](https://www.smartmontools.org/browser/trunk/smartmontools/smartctl.8.in)。

单击 **START** 开始测试。 根据您选择的测试类型，测试可能需要一些时间才能完成。 当测试发现问题时，TrueNAS 会生成警报。

转到存储（**Storage**） > 磁盘（**Disks**），展开一个条目，然后单击 S.M.A.R.T. 检测结果(**S.M.A.R.T. TEST RESULTS**)。 在 [Shell](https://www.truenas.com/docs/core/administration/shell/) 中，使用 `smartctl` 和驱动器名称，例如：`smartctl -l selftest /dev/ada0`。

## 自动S.M.A.R.T.检测

安排定时 S.M.A.R.T. 测试，转到任务（**Tasks**）> S.M.A.R.T. 测试（**S.M.A.R.T. Tests**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksSMARTTestsAdd.png)

| 项目名称                  | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 所有磁盘（**All Disks**） | 设置启用所有磁盘的 S.M.A.R.T. 测试。 保持未勾选以选择要测试的*磁盘*。 |
| 选择磁盘（**Disks**）     | 选择要监控的磁盘。                                           |
| 类型（**Type**）          | 选择测试类型。 有关每种类型的说明，请参阅 [smartctl(8)](https://www.smartmontools.org/browser/trunk/smartmontools/smartctl.8.in)。 某些类型会降低性能或使磁盘脱机。 |
| 描述（**Description**）   | 输入有关此 S.M.A.R.T.测试的信息。                            |
| 日程安排（**Schedule**）  | 测试运行的时间。 选择预设或选择*自定义*以打开高级调度程序。  |

选择要测试的磁盘、要运行的测试类型和任务计划。

注意：SMART 测试可能会导致磁盘离线！ 避免安排 S.M.A.R.T. 与清理或重新同步操作同时进行测试。

当测试必须在非常特定的计划上运行时，将其设置为自定义以打开高级计划程序。

#### 高级调度

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

##### CRON 语法示例

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

保存的计划出现在任务（**Tasks**）> S.M.A.R.T. 测试（**S.M.A.R.T. Tests**）列表。

要验证计划是否已保存，您可以打开shell并输入 `smartd -q showtests`。

## 服务选项

必须启用服务才能运行自动 S.M.A.R.T 测试。

当磁盘由 RAID 控制器控制时，请禁用 S.M.A.R.T. 。 控制器会监控 S.M.A.R.T. 并在测试失败时将磁盘标记为预测性故障。

启动 S.M.A.R.T. 服务，转到服务（**Services**）并切换 S.M.A.R.T.. 要在 TrueNAS 启动过程中启动服务，请设置自动启动（**Start Automatically**）。

通过单击编辑按钮来配置 S.M.A.R.T. 服务。

![](https://www.truenas.com/docs/images/CORE/12.0/smartoptions12.png)

| 项目名称                       | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| 检查间隔（**Check Interval**） | 定义 [smartd](https://www.freebsd.org/cgi/man.cgi?query=smartd&manpath=FreeBSD+11.1-RELEASE+and+Ports) 唤醒的分钟数，并检查是否有任何测试配置为运行。 |
| 电源模式（**Power Mode**）     | 仅在选择从不时才执行测试。                                   |
| 差异（**Difference**）         | 输入摄氏度数。 SMART 报告自上次报告以来驱动器的温度是否变化了 N 摄氏度。 |
| 信息（**Informational**）      | 输入以摄氏度为单位的阈值温度。 如果温度高于阈值，SMART 将发送日志级别为 LOG_INFO 的消息。 |
| 关键（**Critical**）           | 输入以摄氏度为单位的阈值温度。 如果温度高于阈值，SMART 将发送日志级别为 LOG_CRIT 的消息并发送电子邮件。 |

更改任何设置后不要忘记单击 **SAVE**。

