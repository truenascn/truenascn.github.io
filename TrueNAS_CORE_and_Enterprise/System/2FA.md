# 两步验证（2FA）

为了提高安全性，使用两步身份验证是非常有必要的。 TrueNAS 提供双因素身份验证 (2FA) 以确保泄露的管理员 (root) 密码无法单独用于访问管理员界面。 为了使用 2FA，需要安装了谷歌身份验证器（ **Google Authenticator**） 的移动设备。

双重身份验证 (2FA) 是一个额外的安全层，添加到您的系统中以防止有人登录，即使他们知道您的密码。这项额外的安全措施要求您使用随机的 6 位代码验证您的身份，该代码每 30 秒重新生成一次，除非间隔被修改，以在您登录时使用。

**优势**：2FA 提供了额外的安全层：通过要求第二种形式的身份识别，2FA 降低了未经授权的用户可以访问系统的可能性。未经授权的用户将没有验证其登录所需的第二个代码。这样可以提高生产力和灵活性：随着劳动力变得更加移动，员工几乎可以从任何设备或位置安全地访问系统，而不会将敏感信息置于风险之中。

**劣势**：需要应用程序才能访问生成的 2FA 代码。如果 2FA 代码不起作用，或者无法访问 2FA 密码，则无法通过 UI 和 SSH 访问系统（如果已设置该选项）。当带有身份验证应用程序的移动设备不可用时，访问系统终端以绕过 2FA。这需要对系统进行 IPMI 或物理访问。要在终端中解除2FA，请输入：`midclt call auth.twofactor.update ‘{ "enabled":false }'`

## 两步验证选项

**注意：两步验证是基于时间的，需要正确设置系统时间。**

![](https://www.truenas.com/docs/images/CORE/12.0/System2FAEnable.png)

### 用户设置

| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 一次性密码 (OTP) 数字长度（**One Time Password (OTP) Digits**） | 一次性密码中的位数。 默认为 6，这是 Google 的标准 OTP 长度。 在选择之前检查您的应用程序/设备设置。 |
| 间隔（**Interval**）                                         | 每个 OTP 的寿命（以秒为单位）。 默认值为 30 秒。 最短时间为 5 秒。 |
| 窗口期（**Window**)                                          | 将密码有效期扩展到 *Interval* 设置之外。 例如，1表示当前密码前后各1个时段，共3个时段密码有效。 扩展窗口期在高延迟情况下很有用。 |
| 为 SSH 启用两步验证（**Enable Two-Factor Auth for SSH**）    | 为系统 SSH 访问启用 2FA。 我们建议在您使用 UI 成功测试 2FA 之前将此禁用。 |

### 系统生成设置

| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 机密（只读）（**Secret (Read-only)**）                       | 当您第一次启用 2FA 时，TrueNAS 会自动创建机密并用于生成 一次性密码 (OTP)。 |
| 供应 URI（包括机密 - 只读）（**Provisioning URI (includes Secret - Read-only)**） | 用于提供 OTP 的 URI。 TrueNAS 在二维码中对 URI（包含机密）进行编码。 要设置像 Google Authenticator 这样的 OTP 应用程序，请使用该应用程序扫描二维码或在应用程序中手动输入密码。 当您第一次激活 2FA 时，TrueNAS 会自动生成 URI。 |

## 开启两步验证

建议在继续之前设置第二个 2FA 设备作为备份。

转到系统（**System**）> 两步验证（**2FA**）。

单击启用两步验证（**Enable Two Factor Authentication**）并保存（**Save**）。

![](https://www.truenas.com/docs/images/CORE/12.0/System2FAOptionsNoSSH.png)

单击确认（**Confirm**）。

单击显示二维码（**Show QR**）。

![](https://www.truenas.com/docs/images/CORE/12.0/System2FAQRCode.png)

在移动设备上启动 Google 身份验证器（ **Google Authenticator**） 并扫描显示的二维码。

## 使用两步验证登录TrueNAS

启用 2FA 会更改 TrueNAS Web 界面和 SSH 登录的登录过程：

### 使用两步验证登录TrueNAS Web界面

登录页面会在输入用户登陆密码的下方要求输入验证码。 如果没有立即出现，请尝试刷新浏览器缓存。
使用 root 用户名，密码，移动设备上的代码（完整无空格）登陆。

![](https://www.truenas.com/docs/images/CORE/12.0/Login2FA.png)

### 使用两步验证登录TrueNAS SSH

确认在系统（**System**）> 两步验证（**2FA**）中勾选了为 SSH 启用双因素身份验证。

转至服务（**Services**） > **SSH** 并编辑服务。 设置使用 root 密码登录(**Log in with root password**)并保存。 切换 SSH 服务并等待状态显示它正在运行(**Running**)。

在您的移动设备上打开 Google 身份验证应用。

打开终端窗口并使用系统主机名或 IP 地址、使用 root 用户名，密码，移动设备上的代码（完整无空格）通过 SSH 进入系统。

![](https://www.truenas.com/docs/images/CORE/SSHConnect2FA.png)









