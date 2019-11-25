![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# SQL服务器中Sharepoint数据库的Pesky Special Principal'dbo'错误
#### Pesky Special Principal ‘dbo’ Error For Sharepoint Database In SQL Server
**发布-日期: 2016年8月3日 (评论)**

![#](images/principal-dbo-error-a.png?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
如果你打开了这篇文章，想必你尝试更新或编辑Sharepoint配置数据库的权限时，你已经看到了那个讨厌的“…cannot use the special principal ‘dbo’”错误了吧，很讨厌对吧？

比起给你一个关于Sharepoint，Powershell和SQL Server之间的Microsoft安全体系结构的冗长详细描述，为什么我不告诉你一个可能对你有用的快速修复呢？

你需要删除Sharepoint配置数据库的现有数据库所有者，然后更新权限。 就像你平常做的一样......没什么特别的。
这次我在使是在AlwaysOn的SQL Server 2014环境中进行了Sharepoint 2013配置。我正在使用名为“MyDomain”的示例域和名为MyDomainSP_Admin的Sharepoint服务帐户 。

Sharepoint的配置数据库是SharePoint2013_Config。


## English
If you arrived here then you’ve already seen that pesky “…cannot use the special principal ‘dbo’” error whenever you try to update, or edit permissions for the Sharepoint Config Database. OMG annoying right??!!

Rather than giving you a long drawn-out verbose description of the Microsoft Security Architecture between Sharepoint, Powershell, and SQL Server, why don’t I just tell you a quick fix that might work for you.

You’ll need to remove the existing Database Owner of your Sharepoint Configuration Database then update the permissions afterwords. Just as you normally would… Nothing special.
In this scenario I have a Sharepoint 2013 configuration on an environment with SQL Server 2014 using AlwaysOn. I’m using an example domain called ‘MyDomain’, and a Sharepoint Service Account called MyDomainSP_Admin. The Sharepoint Config database is SharePoint2013_Config.


![#](images/principal-dbo-error-a.png?raw=true "#")

这是我用来快速更新DB_Owner，Sharepoint_Shell_Access和SPDataAccess权限的逻辑（logic）。 尽管在这种情况下我很清楚权限层次结构，但我在这个例子中做了所有三个以保持一致性。

Here’s the logic I used to quickly update the permissions for DB_Owner, Sharepoint_Shell_Access, and SPDataAccess. I’m doing all three for consistency in this example even though I’m well aware of the permissions hierarchy in this situation.


---
## Logic
```SQL
use master;
set nocount on
exec [Sharepoint2013_Config]..sp_changedbowner 'sa';
 
use [Sharepoint2013_Config];
create user [MyDomainsp_admin] for login [MyDomainsp_admin]
alter role [db_owner] add member [MyDomainsp_admin]
alter role [sharepoint_shell_access] add member [MyDomainsp_admin]
alter role [spdataaccess] add member [MyDomainsp_admin]
go


```

![#](images/principal-dbo-error-b.png?raw=true "#")

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

