# 同步任务

数据通常需要复制到另一个系统进行备份或迁移到新系统。 一种快速且安全的方法是使用 [rsync](https://rsync.samba.org/)。 这里声明 TrueNAS 系统用于同步任务的双方：主机和远程。

## 基本要求

同步需要一个包含主机或远程系统上所需数据的[数据集](https://www.truenas.com/docs/core/storage/pools/datasets/)。 Rsync 提供推送或拉取数据的能力。使用 rsync 推送时，数据从主机系统复制到远程系统。使用 rsync 拉取时，数据从远程系统拉取并放在主机系统上。

远程系统必须激活 rsync 服务。

## 创建同步任务

转至任务（**Tasks**） > 同步任务（**Rsync Tasks**）并单击添加。有两种主要的同步模式（*Rsync Mode*s）：模块式（*Module*）和 SSH。每种模式的要求是不同的。

### 模块式同步

#### 模块要求

在主机系统上创建同步任务之前，您必须在远程系统上创建一个模块。 当 TrueNAS 是远程系统时，转到服务（**Services**）页面并单击 rsync 服务的编辑按钮来创建模块。 单击 Rsync 模块（**Rsync Module**）选项卡，然后单击添加（**ADD**）。 具体的创建说明位于 [Rsync 服务](https://www.truenas.com/docs/core/tasks/rsync/#rsync-service-and-modules)部分。

#### 创建过程

登录主机系统界面，进入任务（**Tasks**） > 同步任务（**Rsync Tasks**），点击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksRsyncTasksAddModeModule.png)

选择要用于 rsync 任务的源数据集和运行 rsync 任务的用户帐户。 选择 rsync 任务的方向。

为 rsync 任务选择一个调度计划。 如果您需要自定义计划，请选择自定义（**Custom**）。

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



接下来，输入远程主机 IP 地址或主机名。 当远程主机上的用户名不同时，使用格式 username@remote_host。 在 Rsync 模式下拉菜单中选择模块。 输入与远程系统上显示的完全相同的远程模块名称。

根据您的特定需求配置其余选项。

#### 选项说明

##### 源选项

| 项目名称                | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 路径（**Path**）        | 浏览到要复制的路径。 FreeBSD 文件路径限制适用。 其他操作系统可能有不同的限制，这可能会影响它们如何用作源或目标。 |
| 用户（**User**）        | 选择运行 rsync 任务的用户。 所选用户必须具有写入远程主机上指定目录的权限。 |
| 方向（**Direction**）   | 将数据流定向到远程主机。 在 *push* 期间，数据集传输到远程模块。 在*pull*期间，数据集存储来自远程系统的文件。 |
| 描述（**Description**） | 输入这个同步任务的描述。                                     |

##### 调度计划

| 项目名称              | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| 日程（**Schedule**）  | 选择计划预设或选择自定义以打开高级计划程序。                 |
| 递归（**Recursive**） | 设置为包括指定目录的所有子目录。 未设置时，仅包含指定的目录。 |

##### 远程选项

| 项目名称                    | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 远程主机（**Remote Host**） | 输入将存储副本的远程系统的 IP 地址或主机名。 如果远程主机上的用户名不同，则使用格式 username@remote_host。 |
| 同步模式（**Rsync Mode**）  | 选择使用 rsync 服务器的自定义远程模块或为 rsync 任务使用 SSH 配置。 |

##### 更多选项

| 项目名称                                         | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| 时间（**Times**）                                | 设置为保留文件的修改时间。                                   |
| 压缩（**Compress**）                             | 设置以减少要传输的数据大小。 推荐用于慢速连接。              |
| 存档（**Archive**）                              | 勾选后，rsync 递归运行，保留符号链接、权限、修改时间、组和特殊文件。 以 root 用户身份运行时，所有者、设备文件和特殊文件也会被保留。 相当于将标志`-rlptgoD` 传递给rsync。 |
| 删除（**Delete**）                               | 删除源目录中不存在的目标目录中的文件。                       |
| 静默（**Quiet**）                                | 设置为禁止来自远程服务器的信息性消息。                       |
| 保留权限（**Preserve Permissions**）             | 设置为保留原始文件权限。 当用户设置为 root 时，这很有用。    |
| 保留扩展属性（**Preserve Extended Attributes**） | [扩展属性](https://en.wikipedia.org/wiki/Extended_file_attributes) 会被保留，但必须得到两个系统的支持。 |
| 延迟更新（**Delay Updates**）                    | 设置为将每个更新文件中的临时文件保存到目录，直到所有传输的文件都重命名结束，传输才会结束。 |
| 辅助参数（**Auxiliary Parameters**）             | 要包含的其他 [rsync(1)](https://rsync.samba.org/ftp/rsync/rsync.html) 选项。 按 Enter 分隔条目。 注意：“*”字符必须用反斜杠 (\*.txt) 转义或在单引号 ('*.txt') 内使用。 |
| 启用（**Enabled**）                              | 启用此同步任务。 取消勾选则代表禁用此 rsync 任务而不删除它。 |

模块式同步模式需要向 **Remote** 部分添加一个字段：

远程模块名称（**Remote Module Name** ）: 必须在rsync服务器的[rsyncd.conf(5)](https://www.samba.org/ftp/rsync/rsyncd.conf.html)或 另一个系统的 Rsync 模块中定义至少一个模块。

取消勾选启用（**Enabled**）将禁用任务计划。 您仍然可以保存同步任务并手动运行它。

### SSH同步

远程系统必须启用 SSH。 要在 TrueNAS 中启用 SSH，请转到服务（**Services**）并切换 **SSH**。

主机系统需要建立到远程的 SSH 连接以执行 rsync 任务。 要创建连接，请转至系统（**System**） > SSH 连接（**SSH Connections**）并单击添加（**ADD**）。 配置半自动连接并将私钥设置为生成新的。

#### 使用终端设置SSH

要使用命令行，请转至主机系统上的 Shell。 当 rsync 任务由非 root 的 TrueNAS 帐户管理时，输入 `su - {USERNAME}`，其中 `{USERNAME}` 是运行 rsync 任务的 TrueNAS 用户帐户。 输入 `ssh-keygen -t rsa` 以创建密钥对。 当提示输入密码时，按 Enter 键而不设置密码（密码会破坏自动化任务）。 这是运行命令的示例：

```
truenas# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:NZMgbuPvTHeEqi3SA/U5wW8un6AWrx8ZsRQdbJJHmR4 tester@truenas.local
The key's randomart image is:
+---[RSA 2048]----+
|      . o=o+     |
|     . .ooE.     |
|      +.o==.     |
|     o.oo+.+     |
|     ...S+. .    |
|    . ..++o.     |
|     o oB+. .    |
|    . =Bo+.o     |
|     o+==oo      |
+----[SHA256]-----+
```

默认公钥位置是 ~/.ssh/id_rsa.pub。 输入 `cat ~/.ssh/id_rsa.pub` 查看密钥并复制文件内容。 在帐户（**Accounts**） > 用户（**Users**）中将其复制到远程系统上的相应用户帐户。 单击 编辑（**EDIT** ）并将密钥粘贴到 SSH Public Key 中。

接下来，使用 `ssh-keyscan` 将主机密钥从远程系统复制到主机系统用户的 .ssh/known_hosts 目录。 在主机系统上，打开 Shell 并输入 `ssh-keyscan -t rsa {remoteIPaddress} >> {userknown_hostsDir}` 其中 `{remoteIPaddress}` 是远程系统 IP 地址，`{userknown_hostsDir}` 是主机系统上的 known_hosts 目录。 示例：`ssh-keyscan -t rsa 192.168.2.6 >> /root/.ssh/known_hosts`。

#### 创建过程

进入任务（**Tasks**） > 同步任务（**Rsync Tasks**），点击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/TasksRsyncTasksAddModeSSH.png)

首先通过在 Rsync 模式下拉列表中选择 SSH 并输入端口号和远程路径来配置 SSH 设置。

接下来，定义用于 rsync 任务的源数据集并选择一个用户帐户。 用户必须与 [SSH 连接](https://www.truenas.com/docs/core/system/systemssh/)用户名相同。

为 rsync 任务选择一个方向，推或拉，然后定义任务计划。 如果您需要自定义计划，请选择自定义。

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

接下来，输入远程主机 IP 地址或主机名。 如果远程主机上的用户名不同，则使用格式 username@remote_host。 根据您的特定需求配置其余选项。

#### 选项说明

##### 源选项

| 项目名称                | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 路径（**Path**）        | 浏览到要复制的路径。 FreeBSD 文件路径限制适用。 其他操作系统可能有不同的限制，这可能会影响它们如何用作源或目标。 |
| 用户（**User**）        | 选择运行 rsync 任务的用户。 所选用户必须具有写入远程主机上指定目录的权限。 |
| 方向（**Direction**）   | 将数据流定向到远程主机。 在 *push* 期间，数据集传输到远程模块。 在*pull*期间，数据集存储来自远程系统的文件。 |
| 描述（**Description**） | 输入这个同步任务的描述。                                     |

##### 调度计划

| 项目名称              | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| 日程（**Schedule**）  | 选择计划预设或选择自定义以打开高级计划程序。                 |
| 递归（**Recursive**） | 设置为包括指定目录的所有子目录。 未设置时，仅包含指定的目录。 |

##### 远程选项

| 项目名称                    | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 远程主机（**Remote Host**） | 输入将存储副本的远程系统的 IP 地址或主机名。 如果远程主机上的用户名不同，则使用格式 username@remote_host。 |
| 同步模式（**Rsync Mode**）  | 选择使用 rsync 服务器的自定义远程模块或为 rsync 任务使用 SSH 配置。 |

##### 更多选项

| 项目名称                                         | 描述                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| 时间（**Times**）                                | 设置为保留文件的修改时间。                                   |
| 压缩（**Compress**）                             | 设置以减少要传输的数据大小。 推荐用于慢速连接。              |
| 存档（**Archive**）                              | 勾选后，rsync 递归运行，保留符号链接、权限、修改时间、组和特殊文件。 以 root 用户身份运行时，所有者、设备文件和特殊文件也会被保留。 相当于将标志`-rlptgoD` 传递给rsync。 |
| 删除（**Delete**）                               | 删除源目录中不存在的目标目录中的文件。                       |
| 静默（**Quiet**）                                | 设置为禁止来自远程服务器的信息性消息。                       |
| 保留权限（**Preserve Permissions**）             | 设置为保留原始文件权限。 当用户设置为 root 时，这很有用。    |
| 保留扩展属性（**Preserve Extended Attributes**） | [扩展属性](https://en.wikipedia.org/wiki/Extended_file_attributes) 会被保留，但必须得到两个系统的支持。 |
| 延迟更新（**Delay Updates**）                    | 设置为将每个更新文件中的临时文件保存到目录，直到所有传输的文件都重命名结束，传输才会结束。 |
| 辅助参数（**Auxiliary Parameters**）             | 要包含的其他 [rsync(1)](https://rsync.samba.org/ftp/rsync/rsync.html) 选项。 按 Enter 分隔条目。 注意：“*”字符必须用反斜杠 (\*.txt) 转义或在单引号 ('*.txt') 内使用。 |
| 启用（**Enabled**）                              | 启用此同步任务。 取消勾选则代表禁用此 rsync 任务而不删除它。 |

SSH Rsync 模式的其他选项：

远程 SSH 端口（**Remote SSH Port**）：输入远程系统的 SSH 端口号。 默认情况下，TrueNAS 中保留 22。
远程路径（**Remote Path**）：浏览到要与之同步的远程主机上的现有路径。 最大路径长度为 255 个字符。
验证远程路径（**Validate Remote Path**）：设置为在定义的远程路径不存在时自动创建。

取消勾选（**Enabled**）可以禁用任务计划而不删除配置。 您仍然可以通过转到任务（**Tasks**） > 同步任务（**Rsync Tasks**）并单击 > ，然后单击“立即运行”（**RUN NOW**）来运行同步任务。

## Rsync 服务和模块

相关系统服务未开启时，同步任务不起作用。 要打开 rsync 服务，请转到服务（**Services**）并切换 rsync。 要在 TrueNAS 启动时激活服务，请设置自动启动（**Start Automatically**）。

单击编辑（**EDIT**）以配置服务。 rsync 配置有两个部分：基本配置选项和 Rsync 模块创建和管理。

### 配置

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesRsyncConfigure.png)

| 项目名称                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| TCP端口（**TCP Port**）              | rsyncd 会侦听此端口。                                        |
| 辅助参数（**Auxiliary Parameters**） | 输入来自 [rsyncd.conf(5)](https://www.samba.org/ftp/rsync/rsyncd.conf.html) 的其他参数。 |

除非需要进行特定更改，否则请使用默认设置。 更改任何设置后不要忘记单击“保存”（**SAVE**）。

### Rsync模块

此处列出了所有创建的模块。 要创建新模块，请单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesRsyncModuleAdd.png)

#### 通用参数

| 项目名称            | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| 名称（**Name**）    | 与 rsync 客户端请求的名称匹配的模块名称。                    |
| 路径（**Path**）    | 浏览到池或数据集以存储接收到的数据。                         |
| 注释（**Comment**） | 描述这个模块。                                               |
| 启用（**Enabled**） | 激活此模块以与 Rsync 一起使用。 取消设置此字段以停用模块而不完全删除它。 |

#### 连接参数

| 项目名称                          | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| 访问模式（**Access Mode**）       | 选择此 rsync 模块的权限。                                    |
| 最大连接数（**Max Connections**） | 与此模块的最大连接数。 0 是无限的。                          |
| 用户（**User**）                  | 在与此模块之间传输文件期间运行 rsync 命令的 TrueNAS 用户帐户。 |
| 组（**Group**）                   | 在与此模块之间传输文件期间运行 rsync 命令的 TrueNAS 组帐户。 |
| 白名单主机（**Hosts Allow**）     | 来自 [rsyncd.conf(5)](https://www.samba.org/ftp/rsync/rsyncd.conf.html)。 与连接客户端的主机名和 IP 地址匹配的模式列表。 如果没有模式匹配，连接将被拒绝。 按 Enter 分隔条目。 |
| 黑名单主机（**Hosts Deny**）      | 来自 [rsyncd.conf(5)](https://www.samba.org/ftp/rsync/rsyncd.conf.html)。 与连接客户端的主机名和 IP 地址匹配的模式列表。 当模式匹配时，连接被拒绝。 按 Enter 分隔条目。 |

#### 其它选项

| 项目名称                             | 描述                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| 辅助参数（**Auxiliary Parameters**） | 输入来自 [rsyncd.conf(5)](https://www.samba.org/ftp/rsync/rsyncd.conf.html) 的任何其他参数。 |

定义主机白名单（**Hosts Allow**）列表后，只有列表中的 IP 和主机名才能连接到模块。

要编辑或删除模块，请转至 Rsync 模块列表并单击条目的 >。

