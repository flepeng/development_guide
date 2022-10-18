# 开发指南

## MySQL

- [MySQL 开发指南](MySQL/MySQL%20开发指南.md)：MySQL 数据库的规范规范，主要参考了阿里的 JAVA 开发规范中的 MySQL 规范以及一些 git 上总结的规范。


## Python

- [Python 项目通用目录结构](Python/Python%20项目通用目录结构.md)：开发 Python 项目通用的目录结构


## frontend_backend_separation

- [RESTful 设计指南](frontend_backend_separation/RESTful%20设计指南.md)
- [前后端分离开发规范](frontend_backend_separation/前后端分离开发规范.md)


## 各类开发环境缩写


英文缩写 | 英文 | 中文 
--- | --- | --- 
DEV | development | 开发 
SIT | System Integrate Test | 系统整合测试（内测）
UAT | User Acceptance Test | 用户验收测试
PET | Performance Evaluation Test | 性能评估测试（压测）
SIM | simulation | 仿真
PRD/PROD | production | 产品/正式/生产


- 开发环境(DEV)：开发环境是程序猿们专门用于开发的服务器，配置可以比较随意，为了开发调试方便，一般打开全部错误报告。

- 测试环境(UAT)：一般是克隆一份生产环境的配置，一个程序在测试环境工作不正常，那么肯定不能把它发布到生产机上。

- 生产环境(PROD)：是指正式提供对外服务的，一般会关掉错误报告，打开错误日志。可以理解为包含所有的功能的环境，任何项目所使用的环境都以这个为基础，然后根据客户的个性化需求来做调整或者修改。

- 三个环境也可以说是系统开发的三个阶段：开发->测试->上线，其中生产环境也就是通常说的真实环境。

- UAT环境：UAT，(User Acceptance Test),用户接受度测试 即验收测试，所以UAT环境主要是用来作为客户体验的环境。

- 仿真环境：顾名思义是和真正使用的环境一样的环境（即已经出售给客户的系统所在环境，也成为商用环境），所有的配置，页面展示等都应该和商家正在使用的一样，差别只在环境的性能方面。