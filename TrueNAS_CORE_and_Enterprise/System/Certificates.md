# 证书管理

默认情况下，TrueNAS 配备了内部自签名证书，可以对 Web 界面进行加密访问。 您可以通过导航到系统（**System**） > 证书（**Certificates**）并单击添加来导入或创建证书或签名请求。 输入证书的名称，然后选择类型。 四个选项是内部证书（**Internal Certificate**）、证书颁发请求 (CSR)（**Certificate Signing Request(CSR)**）、导入证书（**Import Certificate**）和导入证书签名请求（**Import Certificate Signing Request**）。 每种类型的过程略有不同。 根据您所需的类型，使用下面的选项卡跳转到相应的部分。

## 内部证书

### 标识符和类型

选择内部证书作为类型。

如果需要，您可以为 CA 选择一个配置文件。 选择配置文件会自动设置某些选项，例如密钥类型、密钥长度、摘要算法等。 如果您想手动设置每个选项，请不要从配置文件下拉列表中选择配置文件。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateIdentifierType.png)

### 证书选项

1、从下拉列表中选择密钥类型（**Key Type**）。我们推荐使用 RSA 密钥类型。
2、选择密钥长度（**Key Length**）。出于安全原因，我们建议至少为 2048。
3、选择摘要算法（**Digest Algorithm**）。我们推荐 SHA256。
4、以天为单位输入 CA 的生命周期（**Lifetime**）以设置 CA 的有效期。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateCertificateOptions.png)

### 填写通用名称

1、输入国家（**Country**）、地区（**Locality**）、组织单位（**Organizational Unit**）（可选）、通用名称（**Common Name**）、州（**State**）、组织（**Organization**）、电子邮件（**Email**）和备用名称（**Subject Alternate Names**）来填写证书的信息。
2、通用名称是[完全限定的主机名 (FQDN)](https://kb.iu.edu/d/aiuv)，并且在证书链中必须是唯一的。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateCertificateOptions.png)

### 基本约束

1、如果您想要基本约束，请勾选上**Basic Constraints**，然后会出现更多选项。
2、设置路径长度（**Path Length**）以确定在有效证书路径中可以跟随此证书的非自行颁发的中间证书的数量。输入 0 允许在证书路径中跟随单个附加证书。
3、选择 **Basic Constraints Config**。您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateBasicConstraints.png)

### 权限密钥标识符

如果您需要一个权限密钥标识符，请将其设置为已启用，然后选择权限密钥配置。 您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateAuthorityKeyIdentifier.png)

### 密钥算法

扩展密钥算法通常用于终端实体证书。

1、如果要使用扩展密钥算法，请勾选**Extended Key Usage**，然后从下拉列表中选择公钥的用法。 您可以在下拉列表中选择多个用途。
2、如果您想将此扩展标识为证书的关键，请启用关键扩展（**Critical Extension**）。 如果 Usages 包含 ANY_EXTENDED_KEY_USAGE，则不要启用关键扩展。

注意：同时使用 Extended Key Usage 和 Key Usage 扩展要求证书的用途与这两个扩展一致。 有关更多详细信息，请参阅 [RFC 3280，第 4.2.1.13 节](https://www.ietf.org/rfc/rfc3280.txt)。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCertificateKeyUsage.png)

## 证书颁发请求

### 标识符和类型

选择证书签名请求作为类型。
如果需要，您可以为 CA 选择一个配置文件。 选择配置文件会自动设置某些选项，例如密钥类型、密钥长度和摘要算法。 如果您想手动设置选项，请不要从配置文件下拉列表中选择配置文件。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSAIdentifierType.png)

### 证书选项

从下拉列表中选择密钥类型。 我们推荐使用 RSA 密钥类型。
选择摘要算法。 我们推荐 SHA256。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSACertificateOptions.png)

### 填写通用名称

1、输入国家（**Country**）、地区（**Locality**）、组织单位（**Organizational Unit**）（可选）、通用名称（**Common Name**）、州（**State**）、组织（**Organization**）、电子邮件（**Email**）和备用名称（**Subject Alternate Names**）来填写证书的信息。
2、通用名称是[完全限定的主机名 (FQDN)](https://kb.iu.edu/d/aiuv)，并且在证书链中必须是唯一的。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSACertificateSubject.png)

### 基本约束

1、如果您想要基本约束，请勾选上**Basic Constraints**，然后会出现更多选项。
2、设置路径长度（**Path Length**）以确定在有效证书路径中可以跟随此证书的非自行颁发的中间证书的数量。输入 0 允许在证书路径中跟随单个附加证书。
3、选择 **Basic Constraints Config**。您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSABasicConstraints.png)

### 权限密钥标识符

如果您需要一个权限密钥标识符，请将其设置为已启用，然后选择权限密钥配置。 您可以从下拉列表中选择多个。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSAAuthorityKeyIdentifier.png)

### 密钥算法

扩展密钥算法通常用于终端实体证书。

1、如果要使用扩展密钥算法，请勾选**Extended Key Usage**，然后从下拉列表中选择公钥的用法。 您可以在下拉列表中选择多个用途。
2、如果您想将此扩展标识为证书的关键，请启用关键扩展（**Critical Extension**）。 如果 Usages 包含 ANY_EXTENDED_KEY_USAGE，则不要启用关键扩展。

注意：同时使用 Extended Key Usage 和 Key Usage 扩展要求证书的用途与这两个扩展一致。 有关更多详细信息，请参阅 [RFC 3280，第 4.2.1.13 节](https://www.ietf.org/rfc/rfc3280.txt)。

![](https://www.truenas.com/docs/images/CORE/12.0/CreateCSAKeyUsage.png)

## 导入证书

### 标识符和类型

选择导入证书作为类型。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCertificateIdentifierType.png)

### 证书选项

如果要导入系统上已有的 CSR，请勾选此系统上存在的 CSR（**CSR exists on this system**），然后从下拉列表中选择要使用的 CSR。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCertificateCertificateOptions.png)

### 证书主体

复制要导入的 CA 的证书并将其粘贴到“证书”（**Certificate**）字段中。
在可用时粘贴与证书关联的私钥（**Private Key**）。 提供至少 1024 位长的密钥。
输入并确认私钥的密码（**Passphrase**）。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCertificateCertificateSubject.png)

## 导入证书颁发请求

### 标识符和类型

选择导入证书作为类型。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCSAIdentifierType.png)

### 证书主体

复制要导入的证书签名请求的内容并将其粘贴到签名请求字段中。
在可用时粘贴与证书关联的私钥。 提供至少 1024 位长的密钥。
输入并确认私钥的密码。

![](https://www.truenas.com/docs/images/CORE/12.0/ImportCSACertificateSubject.png)

