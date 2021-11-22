# 简单网络管理协议（SNMP）

[SNMP（简单网络管理协议）](https://tools.ietf.org/html/rfc1157)监控网络连接设备是否存在需要管理关注的情况。 TrueNAS 使用 [Net-SNMP](https://sourceforge.net/projects/net-snmp/) 来提供 SNMP。 要配置 SNMP，请转到“服务”（**Services**）页面，找到 **SNMP** 条目，然后单击编辑图标 。

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSNMPOptions.png)

## 字段说明

### 通用选项

| 设置选项                | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| 位置（**Location**）    | 输入系统的位置。                                             |
| 联系方式（**Contact**） | 接收 SNMP 服务消息的电子邮件地址。                           |
| 共同体（**Community**） | 从 公共（**public**） 更改以提高系统安全性。 只能包含字母数字字符、下划线 (`_`)、破折号 (`-`)、句点 (`.`) 和空格。 对于 SNMPv3 网络，这可以留空。 |

### SNMP v3选项

| 设置选项                            | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| 支持SNMP v3（**SNMP v3 Support**）  | 设置为启用对 [SNMP 版本 3](https://tools.ietf.org/html/rfc3410) 的支持。 有关配置详细信息，请参阅 [snmpd.conf(5)](https://net-snmp.sourceforge.io/docs/man/snmpd.conf.html)。 |
| 用户名（**Username**）              | 输入用户名以注册此服务。                                     |
| 授权方式（**Authentication Type**） | 选择一种身份验证方法：`---` 表示无、*[SHA](https://tools.ietf.org/html/rfc4634)* 或 *[MD5](https://tools.ietf.org/ html/rfc1321)* |
| 密码（**Password**）                | 输入至少八个字符的密码。                                     |
| 隐私协议（**Privacy Protocol**）    | 选择隐私协议：`---` 表示无、*[AES](https://tools.ietf.org/doc/tcllib/html/aes.html)* 或 *[DES](https:// tools.ietf.org/doc/tcllib/html/des.html)* |
| 隐私短语（**Privacy Passphrase**）  | 输入单独的隐私密码。 *密码*在留空时使用。                    |

### 其它选项

| 设置选项                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 辅助参数（**Auxiliary Parameters**）                         | 输入任何其他 [snmpd.conf](https://net-snmp.sourceforge.io/docs/man/snmpd.conf.html) 选项。 为每一行添加一个选项。 |
| 通过 SNMP 暴露 zilstat（**Expose zilstat via SNMP**）        | 启用此选项可能会对您的池产生性能影响。                       |
| 日志等级（**Log Level**）                                    | 选择要创建的日志条目数。 选择范围从最少（紧急）**(Emergency)**到最多（调试）**(Debug)**。 |
| 启用网络性能统计（**Enable Network Performance Statistics**） | 在 SNMP 消息中包含 [iftop](https://linuxcommandlibrary.com/man/iftop) 网络性能统计信息。 |

启动 SNMP 服务时，端口 UDP 161 侦听 SNMP 请求。

## 管理信息库 (MIB)

可用的管理信息库 (MIB) 位于 /usr/local/share/snmp/mibs。 该目录包含许多经常在目录中添加或删除的文件。 通过单击 **Shell** 并输入 `ls /usr/local/share/snmp/mibs` 检查系统上的目录。 以下是目录内容的示例：

![](https://www.truenas.com/docs/images/CORE/12.0/ServicesSNMPMibSample.png)

