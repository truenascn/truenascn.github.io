# TrueNAS中文文档



由于中文TrueNAS资料较少，重复度高且涉及的内容浅，为了以后找TrueNAS的资料更加方便，所以我打算直接翻译TrueNAS的官方文档。

这是对我来说，这是一项浩大的工程，所以只能慢慢翻译了。

本人水平不够，非IT专业出身，再加上时间比较紧，翻译有一些错误，如果发现错误或遗漏，希望大家能够通过[联系邮箱](mailto:mail://xkw@xkw.moe)向我反馈。

###### XKW 2021-11-14

---

# 目录

* [前言](README.md)
* [1.TrueNAS Core和Enterprise](TrueNAS_CORE_and_Enterprise/README.md)
    * [1.1.TrueNAS初探](TrueNAS_CORE_and_Enterprise/gettingstarted/README.md)
        * [1.1.1.TrueNAS Core 硬件指南](TrueNAS_CORE_and_Enterprise/gettingstarted/core_hardware_guide.md)
        * [1.1.2.安装TrueNAS](TrueNAS_CORE_and_Enterprise/gettingstarted/INSTALL.md)
        * [1.1.3.控制台设置菜单](TrueNAS_CORE_and_Enterprise/gettingstarted/Console_Setup_Menu.md)
        * [1.1.4.登录到TrueNAS WebUI](TrueNAS_CORE_and_Enterprise/gettingstarted/log_in.md)
        * [1.1.5.存储配置](TrueNAS_CORE_and_Enterprise/gettingstarted/Storage_Configuration.md)
        * [1.1.6.存储共享](TrueNAS_CORE_and_Enterprise/gettingstarted/Sharing_Storage.md)
        * [1.1.7.数据备份](TrueNAS_CORE_and_Enterprise/gettingstarted/Data_Backups.md)
        * [1.1.8.应用](TrueNAS_CORE_and_Enterprise/gettingstarted/Applications.md)
    * [1.2.账户](TrueNAS_CORE_and_Enterprise/Accounts/README.md)
        * [1.2.1.TrueNAS用户组](TrueNAS_CORE_and_Enterprise/Accounts/Groups.md)
        * [1.2.2.TrueNAS用户](TrueNAS_CORE_and_Enterprise/Accounts/Users.md)
    * [1.3.系统设置](TrueNAS_CORE_and_Enterprise/System/README.md)
        * [1.3.1.通用设置](TrueNAS_CORE_and_Enterprise/System/General/README.md)
            * [1.3.1.1.通用设置页面](TrueNAS_CORE_and_Enterprise/System/General/Setting.md)
            * [1.3.1.2.系统备份设置](TrueNAS_CORE_and_Enterprise/System/General/System_Config_Backups.md)
            * [1.3.1.3.管理TLS加密](TrueNAS_CORE_and_Enterprise/System/General/Managing_TLS_Ciphers.md)
        * [1.3.2.时间同步](TrueNAS_CORE_and_Enterprise/System/NTP_Servers.md)
        * [1.3.3.启动与引导](TrueNAS_CORE_and_Enterprise/System/Boot/README.md)
            * [1.3.3.1.引导备份](TrueNAS_CORE_and_Enterprise/System/Boot/Boot_Screen.md)
            * [1.3.3.2.启动池镜像](TrueNAS_CORE_and_Enterprise/System/Boot/Mirroring_the_Boot_Pool.md)
        * [1.3.4.高级设置](TrueNAS_CORE_and_Enterprise/System/Advanced.md)
        * [1.3.5.机箱监视](TrueNAS_CORE_and_Enterprise/System/View_Enclosure.md)
        * [1.3.6.电子邮件通知](TrueNAS_CORE_and_Enterprise/System/Email.md)
        * [1.3.7.系统数据集](TrueNAS_CORE_and_Enterprise/System_Dataset.md)
        * [1.3.8.系统报告](TrueNAS_CORE_and_Enterprise/System/Reporting.md)
        * [1.3.9.警告服务](TrueNAS_CORE_and_Enterprise/System/Alert.md)
        * [1.3.10.云服务与云同步](TrueNAS_CORE_and_Enterprise/System/Cloud_Credentials.md)
        * [1.3.11.SSH客户端](TrueNAS_CORE_and_Enterprise/System/SSH.md)
        * [1.3.12.可调参数](TrueNAS_CORE_and_Enterprise/System/Tunables.md)
        * [1.3.13.系统更新](TrueNAS_CORE_and_Enterprise/System/Update.md)
        * [1.3.14.证书颁发机构（CA）](TrueNAS_CORE_and_Enterprise/System/CAs.md)
        * [1.3.15.证书管理](TrueNAS_CORE_and_Enterprise/System/Certificates.md)
        * [1.3.16.密钥管理互通协议（KMIP）](TrueNAS_CORE_and_Enterprise/System/KMIP.md)
        * [1.3.17.故障转移与高可用](TrueNAS_CORE_and_Enterprise/System/Failover_HA.md)
        * [1.3.18.帮助与支持](TrueNAS_CORE_and_Enterprise/System/Support.md)
        * [1.3.19.两步验证](TrueNAS_CORE_and_Enterprise/System/2FA.md)
    * [1.4.任务](TrueNAS_CORE_and_Enterprise/Tasks/README.md)
        * [1.4.1.定时任务](TrueNAS_CORE_and_Enterprise/Tasks/Cron_Jobs.md)
        * [1.4.2.开/关机脚本](TrueNAS_CORE_and_Enterprise/Tasks/Init_Shutdown_Scripts.md)
        * [1.4.3.同步任务](TrueNAS_CORE_and_Enterprise/Tasks/Rsync_Tasks.md)
        * [1.4.4.硬盘检测任务](TrueNAS_CORE_and_Enterprise/Tasks/SMART_TEST.md.md)

