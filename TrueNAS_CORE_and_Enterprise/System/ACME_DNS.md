# 使用ACME DNS验证签发证书

注意：此功能仅在支持开源的 TrueNAS CORE 中可用。

[自动证书管理环境 (ACME)](https://ietf-wg-acme.github.io/acme/draft-ietf-acme-acme.html) 可用于自动颁发和更新证书。 在允许证书自动化之前，用户必须验证域的所有权。

需要 ACME DNS Authenticator 来配置 ACME 证书自动化。 这也需要[证书签名请求](https://www.truenas.com/docs/core/system/certificates/)。

## ACME DNS 身份验证器

转至系统（**System**） > ACME DNS域（**ACME DNS**） 并单击添加（**ADD**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemACMEDNSAdd.png)

输入身份验证器的名称。 这仅用于识别 TrueNAS Web 界面中的身份验证器。 选择 DNS 提供商并配置任何所需的身份验证器属性：

**Route 53**：亚马逊 DNS 网络服务。 需要输入亚马逊账户访问 ID 密钥和秘密访问密钥。 有关生成这些密钥的更多详细信息，请参阅 AWS 文档。

单击提交以注册 DNS 身份验证器并将其添加到 ACME 证书的身份验证器选项列表中。

## 创建 ACME 证书

可以为现有的证书签名请求创建 ACME 证书。 这些证书使用 ACME DNS 身份验证器来确认域所有权，然后自动颁发和续订。 要创建新的 ACME 证书，请转到系统（**System**） > 证书（**Certificates**），单击现有证书签发请求的 选项（右侧的三个点），然后单击创建 ACME 证书（**Create ACME Certificate**）。

![](https://www.truenas.com/docs/images/CORE/12.0/SystemCertificatesAddACMECertificate.png)

| 项目名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 标识符（**Identifier**）                                     | 证书的内部标识符。 只允许使用字母数字字符、破折号 (`-`) 和下划线 (`_`)。 |
| 服务条款（**Terms of Service**）                             | 请接受给定 ACME 服务器的服务条款。                           |
| 证书更新日期（**Renew Certificate Day**）                    | 到期前指定天数会自动续订证书。                               |
| ACME 服务器目录 URI（**ACME Server Directory URI**）         | ACME 服务器目录的 URI。 选择预配置的 URI 或输入自定义 URI。  |
| *域名*的验证器（*域名*动态变化）（**Authenticator for *Domain Name* (*Domain Name* dynamically changes)**） | 验证域的身份验证器。 选择先前配置的 ACME DNS 身份验证器。    |

