# 帮助与支持

有许多选项可以为您的 TrueNAS 安装获得支持。 TrueNAS CORE 用户可以与 TrueNAS 社区互动，回答问题和解决问题，而 TrueNAS Enterprise 硬件客户也可以直接获得 iXsystems 提供的快速有效的支持。

## TrueNAS CORE 支持选项

TrueNAS CORE 用户可以使用许多资源来回答使用问题或对系统配置进行故障排除。 始终建议在软件文档和社区资源中搜索这些问题的答案。

也欢迎用户在项目 Jira 实例中报告错误、投票或建议新的 TrueNAS 功能。

### TrueNAS 社区

[TrueNAS 社区](https://www.truenas.com/community/)是一个活跃的在线资源，用于提出问题、解决问题以及与其他 TrueNAS 用户共享信息。 发帖需要[注册](https://www.truenas.com/community/register/)。 鼓励新用户在发帖前简要[介绍](https://www.truenas.com/community/forums/introductions.25/)自己并查看[论坛规则](https://www.truenas.com/community/threads/forum-rules.45124/)。

[社区资源](https://www.truenas.com/community/resources/)是用户贡献的关于使用 TrueNAS 的方方面面的文章。 它们被组织成广泛的类别，并包含一个社区评级系统，以更好地突出整个社区认为有用的内容。

### 社交媒体

欢迎您使用各种社交媒体平台与其他 TrueNAS 用户建立联系！

- [Reddit](https://www.reddit.com/r/truenas/)
- [推特](https://mobile.twitter.com/freenas)
- [领英](https://www.linkedin.com/groups/3903140/)
- [脸书](https://www.facebook.com/freenascommunity)

### 报告错误

如果您在使用 TrueNAS 时遇到错误或其他问题，请在 [TrueNAS Jira 项目](https://jira.ixsystems.com/projects/NAS/)中创建错误报告。 Web 界面提供了一个无需注销即可报告问题的表单。 建议先搜索项目，看看问题是否已经报告。 在创建错误票证之前，您需要创建一个 [Jira 帐户](https://jira.ixsystems.com/secure/Signup!default.jspa)。

要使用 Web 界面报告问题，请转至系统（**System**） > 支持（**Support**）。

![](https://www.truenas.com/docs/images/CORE/12.0/UIBugReport.png)

输入您的 Jira 用户名和密码以验证您的帐户凭据并解锁提交（**Submit**）按钮。类别下拉菜单有大量选项。选择最适合您遇到问题的类别。

通常建议将调试文件或屏幕截图附加到您的错误票证中，以帮助加快响应速度并找到错误。调试文件始终私下附加到故障单，并在故障单解决后删除。

保持主题简短和信息丰富。拥有简短的描述性主题可以让社区更好地查找和响应您的问题。描述应包含有关问题的更多详细信息。建议保持描述少于三段，并包括重现问题或错误的任何步骤。

### 创建调试文件

TrueNAS Web 界面允许用户将调试信息保存到文本文件中。

转至系统(**System**) > 高级(**Advanced**)（TrueNAS SCALE 中的系统设置(**System Settings**) > 高级(**Advanced**)），然后单击保存调试(**SAVE DEBUG**)。单击继续(**PROCEED**)以生成调试文件。生成调试文件时，您无法单击 Web 界面中的选项。会出现一个对话框显示调试文件的创建进度。

生成调试文件后，TrueNAS 会提示您将其保存到本地系统（例如下载），或者它会自动下载到您指定的下载文件位置。

调试信息由 `freenas-debug` 命令行实用程序收集。信息的副本保存到 /var/tmp/fndebug。

### 建议新功能

想看到 TrueNAS 添加的新功能吗？您可以在 TrueNAS Jira 项目中查看社区提议的功能并为其投票，并提出您的功能建议。要查看社区提议的功能列表，请转到 TrueNAS Jira 项目并搜索开放建议。如果您发现您希望看到实施的建议，请打开该票并单击“人员”部分中的为此问题投票。

![](https://www.truenas.com/docs/images/Contribute/JiraSuggestionVote.png)

要建议新功能，请访问 https://jira.ixsystems.com/projects/NAS/，登录您的 Jira 帐户，然后单击创建。

![](https://www.truenas.com/docs/images/Contribute/JiraSuggestionCreate.png)

输入简短的摘要并描述您希望添加到软件中的新功能。 创建功能建议后，它将进入“收集兴趣”阶段，社区可以在该阶段对该功能进行审查和投票。 在收集到足够的兴趣后，TrueNAS 发布委员会将审查可行性建议，并在软件路线图中找到添加该功能的位置。

## TrueNAS Enterprise 支持选项

除了所有 TrueNAS CORE 支持选项外，如果系统出现问题，从 iXsystems 购买硬件的 TrueNAS Enterprise 客户可以从 iXsystems 获得帮助。

银级和金级支持客户还可以在其硬件上启用主动支持，以便在出现问题时自动通知 iXsystems。 要查找有关可用的不同保修和服务级别协议 (SLA) 选项的更多详细信息，请参阅 https://www.ixsystems.com/support/。

### 许可证信息

许可信息区域包含系统型号（**Model**）、系统序列号（**System Serial**）、附加硬件（**Additional Hardware**）、许可功能（**Features**）、支持合同类型（**Contract Type**）和支持合同到期日期（**Expiration Date**）信息。 还可以将系统标记为生产系统（**production system**）。

![](https://www.truenas.com/docs/images/CORE/12.0/LicenseInformationArea.png)

### 生产系统报告

系统准备好投入生产后，通过选中“这是生产系统”（**This is a production system**）复选框来更新状态，然后单击“更新状态”（**Update Status**）按钮。 这将向支持部门发送一封电子邮件，声明系统已投入生产。 有一个选项可以在此电子邮件中包含调试，这可能有助于将来的支持。

## 配置主动支持

每当系统上的硬件状况需要注意时，主动支持都会通过电子邮件通知 iXsystems。 此功能适用于 iXsystems 银牌和金牌支持客户。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemSupportProactiveEnterprise.png)

请务必添加有效的电子邮件地址和电话号码，以便在出现任何问题时快速通知联系人。

您还可以在系统控制台菜单（Shell 中的 `/etc/netcli`）中切换自动 iXsystesms 支持警报。 在可以切换此选项之前，必须在 TrueNAS 高可用性系统中禁用故障转移。 要在 Web 界面中以管理方式禁用故障转移，请转至系统（**System**） > 故障转移（**Failover**）。

### 提交工单

TrueNAS Enterprise 客户可以通过转到 Web 界面中的系统（**System**） > 支持（**Support**），直接向 iXsystems 支持提交工单。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemSupportContactEnterprise.png)

请务必输入有效的电子邮件和电话号码。 iXsystems 支持使用此信息快速响应和解决问题。您还可以指出系统的当前使用情况并确定问题对系统可用性的重要性。

始终建议附加调试和屏幕截图，以帮助加快诊断和解决问题的速度。有一个信息丰富的主题和描述来简要描述问题以及是否有重现问题的任何步骤也很有帮助。

单击提交（**SUBMIT**）会生成工单并将其发送到 iXsystems。在收集和发送信息时，此过程可能需要几分钟时间。如果在 Proactive Support 处于活动状态时创建票证失败，TrueNAS 会发送电子邮件警报。

创建新票证后，将显示 URL 以供查看或更新更多信息。需要 [iXsystems 支持](https://support.ixsystems.com/)帐户才能查看票证。单击 URL 以登录或注册支持门户。注册时使用与工单提交的电子邮件地址相同的电子邮件地址。

## 联系 iXsystems 

| 联系方式 | 联系信息                                                     |
| -------- | ------------------------------------------------------------ |
| 网站     | [https://support.ixsystems.com](https://support.ixsystems.com/) |
| 电子邮件 | [support@ixsystems.com](mailto://support.ixsystems.com)      |
| 电话     | 周一 - 周五, 上午6:00 至 下午 6:00 太平洋标准时间：1-855-473-7449 按2（仅限美国）；1-408-943-4100 按·2（国际） |
| 电话     | 7x24小时在线：1-855-499-5131（仅限美国）；1-408-878-3140（国际） |





