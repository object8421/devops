
## 基本信息

- 姓名：漆昭红
- 性别：男
- 出生日期：1990-11-26
- 居住地：深圳南山
- 工作年限：三年以上
- 邮箱：Honty77@qq.com
- 手机：18665935526
- 学历：本科
- 专业：软件工程
- 毕业院校：桂林电子科技大学


## 个人素质

1. 对技术有强烈的兴趣，喜欢钻研，能独立分析和解决问题；性格和善，工作积极，有责任心，具备较强的学习能力和良好的工作效率。
1. 有三年以上的工作经验，熟悉软件开发过程和敏捷项目开发经验；有较强的业务理解能力和建模能力；具有面向对象分析（OOA）、设计（OOD）、编程开发（OOP）能力，熟练使用思维导图、PowerDesigner、Visio、ProcessOn等建模工具。
1. 有较强的代码编写能力，遵循编码规范，编写项目相关文档；有良好的代码质量审查、测试和重构的习惯；喜欢关注开源项目，有较强的源码阅读能力。

## 技术要求

1. 熟悉JavaEE开发，具有扎实的Java基础，熟悉JSP/Servlet, IO，多线程，网络编程，缓存机制，消息通信，AOP，MVC等技术知识，了解设计模式与组件技术。
1. 熟悉Spring, Spring MVC, MyBatis, Hibernate, Struts2, Shiro, freemark等主流开发框架。
1. 熟练掌握SQL，熟悉Oracle，MySQL, PostgreSQL, Redis等数据库技术和应用。
1. 有Linux平台的开发和运维经验，熟悉Shell脚本，有一定的Python经验，有阿里云和Windows Azure等云平台的项目管理和部署经过。

1. 熟练掌握HTML/CSS/JS/AJAX/JSON等前端技术；熟悉jQuery/EasyUI, Bootstrap, LESS, SCSS, grunt, seajs, AngularJS等前端框架和构建工具， 具备一定的模块化编程和异步编程的经验。
1. 熟悉Apache, nginx, tomcat等服务器配置和使用， 熟悉memcached, MQ等中间件技术。
1. 熟练使用Git, SVN, JIRA等项目管理软件。

## 工作经验

深圳市聚财在线金融信息服务有限公司

工作时间：

项目名称：聚财宝


### 深圳市凌云新创信息技术有限公司

工作时间：

项目名称：CivicData

[CivicData](http://www.civicdata.com/)是美国Accela公司的一个OpenData项目, 主要为组织机构提供数据存储，管理和数据分析的开放平台。CivicData基于[CKAN](http://ckan.org/)开源平台构建，集成了DataPusher, DataStore, FileStore, Stats, Disqus等插件应用。同时还开发了DataLoader数据加载器和AdminPortal后台管理系统。整个项目基于Windows Azure部署，为了更好地支撑系统的高可用性，配置了反向代理和负载均衡服务器，增加了服务器缓存，使用公司内部的自动化测试框架（ACDP）进行测试，配置newrelic实时监控系统运行状况。

项目架构剖析：

- CKAN, the world’s leading open-source data portal platform. 使用Python编写。
- DataPusher：数据解析插件，用户上传的CSV/Excel文件会自动解析到数据库。
- DataStore：对外提供一套完整的API服务接口。
- FileStore：文件存储插件，使用ptree方式将上传的文件保存在指定目录，也可以配置到云服务器（如Google，Amazon S3）
- DataLoader：数据加载器，为Accela公司的AA项目提供数据加载服务，可以定时去执行任务，将数据写到CKAN数据库。
- AdminPortal：后台管理系统，提供数据管理、数据模板，查询和写入数据，方便组织机构使用。