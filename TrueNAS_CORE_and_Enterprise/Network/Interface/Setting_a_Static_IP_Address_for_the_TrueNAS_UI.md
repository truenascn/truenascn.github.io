# 在TrueNAS用户界面中设置静态IP

**注意：更改 Web 界面使用的网络界面可能会导致与 TrueNAS 的连接丢失！ 修复任何错误配置的网络设置可能需要命令行知识或对 TrueNAS 系统的物理访问。**

## 流程总结

- Web用户界面
  - 网络（**Network**） > 网络接口（**Interfaces**） > 添加（**Add**）或编辑（**Edit**）
    - 在 IP 地址 中输入地址并选择子网掩码。
    - 根据需要添加或删除其他地址。
  - 在应用之前需要测试保存的更改。
    - 会弹出对话框要求临时应用更改。
    - 应用更改后，在永久保存之前的配置之前，您有一段可调整的时间来验证新设置是否有效。
  - 网络（**Network**） > 网络概览（**Network Summary**） 概述了每个接口的配置信息。
- 控制台菜单
  - Physical Interfaces: 选择配置网络接口（其他接口类型的选项类似）
    - Delete interface? `n`
    - Remove interface settings? `n`
    - Configure IPv4?
      - 输入 IP 地址和子网掩码
    - Configure IPv6
      - 输入 IP 地址
    - Configure failover? `n`
  - 保存更改会中断 Web 界面，并且可能需要重新启动系统。

## 设置静态 IP 地址

TrueNAS 可以在 Web 界面或系统控制台菜单中使用静态 IP 地址配置物理网络接口。

建议在此过程中使用 Web 界面。 还有额外的安全功能可以防止保存错误配置的接口设置。

### 使用 Web 界面添加静态 IP 地址

登录到 Web 界面并转到网络（**Network**） > 网络接口（**Interfaces**）。 这包含物理和虚拟网络接口的创建和配置选项。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfaces.png)

您可以在创建或编辑接口时配置静态 IP 地址。

在编辑活动界面之前，必须在 TrueNAS Enterprise 系统上禁用高可用性。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesEdit.png)

在 IP 地址字段中键入所需的地址并选择子网掩码。

多个接口不能是同一子网的成员。 有关详细信息，请参阅[单个子网上的多个网络接口](https://www.ixsystems.com/community/threads/multiple-network-interfaces-on-a-single-subnet.20204/)。 如果在多个接口上设置 IP 地址时出现错误，请检查子网掩码。

根据需要使用按钮添加和删除更多 IP 地址。

为了避免永久保存无效或不可用的设置，网络更改会临时应用。 保存任何接口更改会在网络（**Network**） > 网络接口（**Interfaces**）列表中添加一个对话框以应用这些更改。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkInterfacesChangesPresent.jpg)

您可以调整在网络更改恢复到以前的设置之前测试网络更改的时间。 如果测试成功，另一个对话框允许永久更改网络。

要快速查看系统网络设置，请转至网络（**Network**） > 网络概览（**Network Summary**）。

![](https://www.truenas.com/docs/images/CORE/12.0/NetworkNetworkSummary.png)

## 使用系统控制台菜单为物理接口分配静态 IP 地址

需要连接到物理系统的显示器和键盘才能使用控制台，或者，如果系统硬件允许，您可以连接 IPMI。 系统完全启动时会显示控制台菜单。

![](https://www.truenas.com/docs/images/CORE/ConsoleSetupMenu.png)

使用配置网络接口（**Configure Network Interfaces**）选项将静态 IP 地址添加到物理接口。 其他接口类型具有类似的过程来添加静态 IP 地址。 已为 DHCP 配置的接口将禁用该选项。 在添加静态地址之前，有许多提示需要回答。 此示例显示向接口 igb0 添加静态 IPv4 地址：

```
  Enter an option from 1-11: 1
  1) igb0
  2) igb1
  Select an interface (q to quit): 1
  Delete interface? (y/n) n
  Remove the current settings of this interface? (This causes a momentary disconne
  ction of the network.) (y/n) n
  Configure IPv4? (y/n) y
  Interface name:
  Several input formats are supported
  Example 1 CIDR Notation:
      192.168.1.1/24
  Example 2 IP and Netmask separate:
      IP: 192.168.1.1
      Netmask: 255.255.255.0, /24 or 24
  IPv4 Address:10.238.15.194/22
  Saving interface configuration: Ok
  Configure IPv6? (y/n) n
  Configure failover settings? (y/n) n
  Restarting network: ok
  Restarting routing: ok
```

保存界面配置更改将在系统网络重新启动时中断 Web 界面。 当正在更改的界面也是提供 Web 界面的界面时，可能需要重新启动系统才能使新设置生效并使 Web 界面再次可用。
