# TrueNAS用户

在 TrueNAS 中，用户帐户允许灵活访问共享数据。 一种常见的做法是创建用户并将其分配给组。 这样便可以方便地对大量用户进行有效的权限管理。

**注意：只有 root 用户帐户才能登录 TrueNAS 网页界面。**

当网络使用活动目录（域服务）时，使用目录服务中的[说明](https://www.truenas.com/docs/core/directoryservices/)导入现有帐户信息。 使用 [Active Directory](https://www.truenas.com/docs/core/directoryservices/activedirectory/) 需要在 Windows 中设置 Windows 用户密码。

要查看用户帐户，请转至帐户（**Accounts**） > 用户（**Users**）。

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsUsersList.png)

TrueNAS 默认隐藏所有内置用户。 要查看所有内置用户，请单击齿轮图标，然后单击显示（**SHOW**）。

## 创建用户帐户

要创建新用户，请转至帐户（**Accounts**） > 用户（**Users**）并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/AccountsUsersAdd.png)

### 身份鉴定（Identification）

输入用户的全名（**Full Name**）。 建议从全名中使用简化的用户名（**Username**），但具体取决于您。

电子邮件（**Email**）地址可以与用户帐户相关联。

为用户设置并确认密码。

### 用户 ID 和归属组（User ID and Groups）

设置用户 ID。 TrueNAS 会自动建议用户 ID，从 1000 开始。 建议非内置用户使用1000以上的ID。

默认情况下，TrueNAS 会创建一个与用户同名的新组。 要将用户添加到现有组，请取消设置新建组（**New Primary Group**）并从主要组下拉列表中选择现有组。 此外，还可以使用辅助组（**Auxiliary Groups**）下拉菜单将用户同时添加到其他组。

### 目录和权限（Directories and Permissions）

创建用户时，主目录路径默认会被设置为 /nonexistent，即不为该用户创建主目录。 要为用户设置主目录，请使用文件浏览器选择路径。 如果目录存在且与用户名匹配，则将其设置为用户主目录。 当路径不以与用户名匹配的子目录结尾时，将创建一个新的子目录。 编辑用户时，此处显示用户主目录的完整路径。

直接在文件浏览器下，可以设置主目录权限。 在TrueNAS中，被创建的用户默认不能更改自己的主目录权限。



### 登陆凭据（Authentication）

可以将公共 SSH 密钥分配给用户以进行基于密钥的身份验证。 只需将公钥粘贴到 SSH 公钥字段中即可。 如果您使用 SSH 公共密钥，最好保留密钥的备份。 单击 **DOWNLOAD SSH PUBLIC KEY** 将粘贴的密钥下载为 .txt 文件。

当禁用密码（**Disable Password**）为是（**YES**）时，用户将不能使用密码登陆。 任何现有密码都将从帐户中删除。 **Lock User** 和 **Permit Sudo** 选项也被删除。 然后，该帐户将不能使用基于密码的服务登录。 例如，禁用密码可防止使用帐户登录 SMB 共享或打开系统上的 SSH 会话。 默认情况下，禁用密码为否。

可以从 Shell 下拉列表中为用户设置特定的 shell：

| Shell     | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| csh       | 用于 UNIX 系统交互的 [C shell](https://docs.freebsd.org/44doc/usd/04.csh/paper.html)。 |
| sh        | [Bourne shell](https://www.in-ulm.de/~mascheck/bourne/v7/)   |
| tcsh      | [增强的 C shell](https://www.tcsh.org/)，包括编辑和名称完成。 |
| bash      | 用于 GNU 操作系统的 [Bourne Again shell](https://www.gnu.org/software/bash/manual/bash.html)。 |
| ksh93     | [Korn shell](http://www.kornshell.com/) 结合了 csh 和 sh 的功能。 |
| mksh      | [MirBSD Korn Shell](https://www.mirbsd.org/mksh.htm)         |
| rbash     | [Restricted bash](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html) |
| rzsh      | [Restricted zsh](https://www.csse.uwa.edu.au/programming/linux/zsh-doc/zsh_14.html) |
| scponly   | [scponly](https://github.com/scponly/scponly/wiki) 将用户的 SSH 使用限制为仅 scp 和 sftp 命令。 |
| zsh       | [Z shell](http://zsh.sourceforge.net/)                       |
| git-shell | [受限的 git shell](https://git-scm.com/docs/git-shell)       |
| nologin   | 在创建系统帐户或创建可以使用共享进行身份验证但无法使用 ssh 登录 TrueNAS 系统的用户帐户时使用。 |

设置锁定用户（**Lock User**）会禁用此帐户的所有基于密码的功能，直到取消设置该选项。

**Permit Sudo** 允许此帐户使用 sudo 命令充当系统管理员。 为提高安全性，推荐禁用此选项。

当用户帐户将使用 Windows 8 或更新的客户端访问存储在 TrueNAS 上的数据时，勾选Microsoft 帐户（**Microsoft Account**）。 这启用了从这些操作系统可用的其他身份验证方法。

默认情况下，启用 Samba 身份验证（**Samba Authentication**）。 这允许使用帐户凭据访问与 SMB 共享的数据。

