# 系统报警服务

警报系统与各种第三方服务集成。 调整警报还有助于针对任何高度敏感的问题对 TrueNAS 进行个性化设置。

## 警报服务

警报服务是 TrueNAS 内置的各种方法，可以通知您系统警报。

**注意：一些警报服务是由第三方提供，可能会对消息或数据使用收费。**

### 添加服务

要添加新的警报服务，请转至系统（**System**） > 警报服务（**Alert Services**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemAlertServicesAdd.png)

| 项目名称            | 描述                               |
| ------------------- | ---------------------------------- |
| 名称（**Name**）    | 新警报服务的名称。                 |
| 启用（**Enabled**） | 取消勾选可以禁用此服务而不删除它。 |
| 类型（**Type**）    | 选择警报服务以显示该服务的选项。   |
| 等级（**Level**）   | 选择严重性级别。                   |

选择类型会添加特定于该警报服务的选项：

#### 亚马逊AWS（AWS）

| 项目名称                  | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| AWS区域（**AWS Region**） | 输入[AWS 账户区域](https://docs.aws.amazon.com/sns/latest/dg/sms_supported-countries.html)。 |
| ARN                       | 用于发布的主题 [Amazon 资源名称 (ARN)](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)。 示例：`arn:aws:sns:us-west-2:111122223333:MyTopic`。 |
| Key ID                    | 链接的 AWS 账户的访问Key ID。                                |
| Secret Key                | 链接的 AWS 账户的访问Secret Key。                            |

#### 电子邮件（Email）

| 项目名称                      | 描述                                           |
| ----------------------------- | ---------------------------------------------- |
| 邮件地址（**Email Address**） | 输入有效的电子邮件地址以接收来自该系统的警报。 |

#### InfluxDB

| 项目名称               | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| 主机地址（**Host**）   | 输入 [InfluxDB](https://docs.influxdata.com/influxdb/) 主机名。 |
| 用户名（**Username**） | 此服务的用户名。                                             |
| 密码（**Password**）   | 输入密码。                                                   |
| 数据库（**Database**） | InfluxDB 数据库的名称。                                      |
| 时序（**Series**）     | 收集点的 InfluxDB 时序名称。                                 |

#### Mattermost

| 项目名称                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| Webhook 网址（**Webhook  URL**） | 输入或粘贴与此服务关联的 [传入 webhook](https://docs.mattermost.com/developer/webhooks-incoming.html) URL。 |
| 用户名（**Username**）           | Mattermost 用户名。                                          |
| 频道（**Channel**）              | 接收通知的 [频道](https://docs.mattermost.com/messaging/managing-channels.html) 的名称。 这会覆盖传入 webhook 设置中的默认通道。 |
| 图标URL（**Icon Url**）          | 用作新消息的个人资料图片的图标文件。 示例：https://mattermost.org/wp-content/uploads/2016/04/icon.png。 需要将 Mattermost 配置为 [覆盖个人资料图片图标](https://docs.mattermost.com/configure/configuration-settings.html#enable-integrations-to-override-profile-picture-icons)。 |

#### OpsGenie

| 项目名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| API Key  | 输入或粘贴 [API 密钥](https://docs.opsgenie.com/v1.0/docs/api-integration)。 通过登录 OpsGenie Web 界面并转到集成/配置集成来查找 API 密钥。 单击所需的集成、设置，然后阅读 API 密钥字段。 |
| API URL  | 默认 OpsGenie API 留空。                                     |

#### Pager Duty

| 项目名称                      | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| 服务密钥（**Service Key**）   | 输入或粘贴此系统的“集成/服务”密钥以访问 [PagerDuty API](https://v2.developer.pagerduty.com/v2/docs/events-api)。 |
| 客户端名称（**Client Name**） | PagerDuty 客户端名称。                                       |

#### Slack

| 项目名称                        | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| Webhook 网址（**Webhook URL**） | 粘贴与此服务关联的 [传入 webhook](https://api.slack.com/incoming-webhooks) URL。 |

#### 简单网络管理协议陷阱（SNMP trap）

| 项目名称                                     | 描述                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| 主机名（**Hostname**）                       | 接收 SNMP 陷阱通知的系统的主机名或 IP 地址。                 |
| 端口（**Port**）                             | 接收 SNMP 陷阱通知的系统上的 UDP 端口号。 默认值为 162。     |
| SNMPv3 安全模式（**SNMPv3 Security Model**） | 启用 SNMPv3 安全模式。                                       |
| SNMP 社区（**SNMP Community**）              | 网络社区字符串。 社区字符串的作用类似于用户 ID 或密码。 具有正确社区字符串的用户可以访问网络信息。 默认是公开的。 有关详细信息，请参阅此有用的 [SNMP 社区字符串教程](https://www.dnsstuff.com/snmp-community-string)。 |

#### Victor Ops

| 项目名称    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| API Key     | 输入或粘贴 [VictorOps API 密钥](https://help.victorops.com/knowledge-base/api/)。 |
| Routing Key | 输入或粘贴 [VictorOps 路由密钥](https://portal.victorops.com/public/api-docs.html)。 |

单击 **SEND TEST ALERT** 测试服务配置。

### 警报设置

要修改默认系统警报，请转至系统（**System**） > 警报设置（**Alert Settings**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemAlertSettings.png)

警报根据类型分为多个部分。 例如，与池相关的警报出现在存储警报部分。

#### 选项

| 项目名称                              | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| 设置报警等级（**Set Warning Level**） | 自定义警报的重要性。 每个重要性级别都有不同的图标和颜色来表示重要性级别：*Info*（所以信息）、*Notice*（需要注意）、*Warning*（警告）、*Error*（错误）、*Critical*（关键错误）（默认）、*Alert* （警报）和 *Emergency*（紧急）。 |
| 设置频率（**Set Frequency**）         | 调整警报通知的发送频率。 将频率设置为 NEVER 则不会发送该警报，但如果触发该警报，该警报仍可显示在 Web 界面中。 选项：*立即*（默认）、*每小时*、*每天*和*从不*。 |

更改这些选项中的任何一个都会影响配置的所有警报服务。

