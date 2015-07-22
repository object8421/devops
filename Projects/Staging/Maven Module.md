# Maven 多模块项目构建

## 模块说明

_说明：本文使用Spring Tool Suite作为开发工具_

### project-root / project-parent

-- 顶级父类模块，命名规则：使用root或parent。

* 构建父模块

[1]. 创建Simple Spring Maven项目: project-root

[2]. pom.xml
```
<!-- 父模块基本信息 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.root</groupId>
<artifactId>project-root</artifactId>
<version>1.0.0</version>

<!-- 重要：将父模块的packaging设置为pom是必须的： pom表示该模块是一个被继承的模块 -->
<!-- 子模块可以设置为jar或war，方便打包 -->
<packaging>pom</packaging>

<!-- 关联子模块/项目 -->
<modules>
	<module>../project-model</module>
	<module>../project-dao</module>
	<module>../project-service</module>
	<module>../project-utils</module>
	<module>../project-middle</module>
	<module>../project-web</module>
	<module>../project-admin</module>
	<module>../project-app</module>
	<module>../project-api</module>
</modules>
```

[3]. 职能说明

* 父模块是一个被继承的模块。负责定义核心功能，子模块直接使用。
* 使用父模块的pom.xml配置来管理通用的Maven依赖。
* 子模块可以根据需要单独配置pom.xml的依赖。
* 原则：最小配置，最少依赖；灵活，通用。

[4]. 包的命名约定[仅供参考]

- com.project.root.model  			-- 基础，通用的Model
- com.project.root.dao 				-- 数据访问层通用接口
- com.project.root.dao.impl  			-- 数据访问层通用实现类
- com.project.root.service   			-- 服务层通用接口
- com.project.root.service.impl   	-- 服务层通用实现类
- com.project.root.controller   		-- 控制层通用类
- com.project.root.annotation 		-- 自定义注解
- com.project.root.interceptor 		-- 自定义拦截器 [配合注解使用]
- com.project.root.aspect				-- AOP [使用AOP处理相关业务]
- com.project.root.component  		-- 业务组件 [系统业务组件]
- com.project.root.factory    		-- 工厂类 [负责核心业务调度]

* 更多与扩展
- com.project.root.xxx.dao			-- 项目中涉及多种业务模块，可以定义扩展包

### project-utils

-- 通用工具包

* 构建工具类模块

[1]. 创建Simple Spring Maven项目: project-utils

[2]. pom.xml

```
<!-- utils模块基本信息：与父模块共用版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.utils</groupId>
<artifactId>project-utils</artifactId>
<packaging>jar</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>
```

[3]. 职能说明

* 工具包继承父模块，主要管理项目中依赖的工具类库
* 自定义工具类应该参考优秀开源项目中的代码编写，如 Apache Commons.
* 工具类不能依赖其他模块，要做好顶层设计。
* 工具类应该具备通用性，标准化，规范化，可扩展性。
* 应该及时优秀工具类的代码和BUG，使之更加高效、安全。

[4]. 包的命名约定[仅供参考]

- com.project.utils.file -- 文件操作
- com.project.utils.lang -- 数据类型
- com.project.utils.http -- HTTP请求
- com.project.utils.text -- 文本操作
- com.project.utils.format -- 格式化

* 更多与扩展
- com.project.utils.xxx -- 以工具类的类型作为包的分类原则。

### project-model

-- 实体POJO模块

* 构建实体模块

[1]. 创建Simple Spring Maven项目: project-model

[2]. pom.xml

```
<!-- 实体POJO模块基本信息：与父模块共用版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.model</groupId>
<artifactId>project-model</artifactId>
<packaging>jar</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>
```

[3]. 职能说明

* 实体层管理与数据库字段的映射。
* 实体层提供对数据安全性的验证。[应该集成Hibernate-validator或Oval验证框架]
* 实体层要统一字段的命名约定。[应该确保风格统一，字段名称简明、通用、一致。]
* 实体层要提供相关的对外方法、构造函数等。[非get/set方法]

[4]. 包的命名约定[仅供参考]

- com.project.model -- 实体POJO。[注意：实体类的命名不要加相关的后缀名称]

* 更多与扩展
- com.project.model.xxx -- 如果有更多的业务类型，可以使用扩展包

### project-dao

-- 数据访问层

* 构建数据层模块

[1]. 创建Simple Spring Maven项目: project-dao

[2]. pom.xml

```
<!-- 数据层模块: 与父模块共享版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.dao</groupId>
<artifactId>project-dao</artifactId>
<packaging>jar</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>

<!-- 添加实体POJO依赖：依赖具有继承性 -->
<dependencies>
	<dependency>
		<groupId>com.project.model</groupId>
		<artifactId>project-model</artifactId>
		<version>1.0.0</version>
	</dependency>
</dependencies>
```

[3]. 职能说明

* 数据访问层与数据库交互。
* 数据访问层只提供相关的数据接口，并确保数据交互的安全性。
* 数据访问层只依赖实体POJO

[4]. 包的命名约定[仅供参考]

- com.project.dao -- 数据接口
- com.project.dao.impl -- 数据实现类 [集成Spring]
- com.project.dao.mapper -- XML文件配置 [使用Mybatis框架]
- mybatis.xml -- 集成到Spring，将mapper文件和实体POJO统一配置和管理

* 更多与扩展
- com.project.xxx.dao -- 如果有更多的业务类型，可以使用扩展包
- com.project.xxx.dao.impl
- com.project.xxx.dao.mapper

### project-service

-- 业务核心：负责实现所有的业务逻辑

* 构建业务模块

[1]. 创建Simple Spring Maven项目: project-service

[2]. pom.xml

```
<!-- 业务核心模块：与父模块共用版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.service</groupId>
<artifactId>project-service</artifactId>
<packaging>jar</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>

<!-- 依赖Model, Dao, Utils 模块 -->
<dependencies>
	<dependency>
		<groupId>com.project.dao</groupId>
		<artifactId>project-dao</artifactId>
		<version>1.0.0</version>
	</dependency>
	<dependency>
		<groupId>com.project.utils</groupId>
		<artifactId>project-utils</artifactId>
		<version>1.0.0</version>
	</dependency>
</dependencies>
```

[3]. 职能说明

* 业务核心模块负责处理项目中的所有业务逻辑。
* 业务核心模块应集成Spring的事务配置。
* 业务核心模块的接口定义必须明确，并执行一个完整的业务功能。
* 业务核心模块的接口请求参数必须是清晰地，明确地。[有必须通过验证框架的验证]
* 业务核心模块必须是安全的。接口调用，请求方式，BUG与异常处理，日志记录必须清楚。
* 业务核心模块的方法职能必须是单一的，以面向接口开发为最基本原则。

[4]. 包的命名约定[仅供参考]

- com.project.service -- 数据接口
- com.project.service.impl -- 数据实现类 [集成Spring]
- com.project.service.rule -- 业务规则定义 [业务规则引擎]
- com.project.service.factory -- 由工厂方法为业务核心提供业务功能调度。[调度引擎]

* 更多与扩展
- com.project.xxx.service -- 如果有更多的业务类型，可以使用扩展包
- com.project.xxx.service.impl

### project-web, project-admin, project-app, project-api

-- WEB应用程序

- *project-web* 		-- PC端应用程序
- *project-admin* 		-- 后台管理程序
- *project-app*			-- 移动客户端程序
- *project-api* 		-- OPEN API服务程序

* 构建WEB模块 [此处以project-web项目为例]

[1]. 创建Simple Spring Maven项目: project-web

[2]. pom.xml

```
<!-- WEB模块：共用父模块版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.web</groupId>
<artifactId>project-web</artifactId>
<name>project-web</name>
<packaging>war</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>

<!-- 项目依赖：Service已经依赖了其他模块 [DAO, MODEL, UTILS] -->
<dependencies>
	<dependency>
		<groupId>com.project.service</groupId>
		<artifactId>project-service</artifactId>
		<version>1.0.0</version>
	</dependency>
</dependencies>
```

[3]. 职能说明

* WEB模块负责与前端交互，提供数据服务与页面视图。
* WEB模块集成Spring配配置。
* WEB模块提供权限管理和安全验证框架。[建议集成Shiro]
* WEB模块响应客户端的方式：JSON数据和页面视图。应定义统一规范的数据服务类[Context]。
* WEB模块要处理请求的安全性，并记录用户请求的相关数据。
* WEB模块要定义统一的规则，如错误处理，错误信息响应。

[4]. 包的命名约定[仅供参考]

- com.project.web.controller -- 控制器
- com.project.web.annotation -- 自定义注解
- com.project.web.interceptor -- 自定义拦截器
- com.project.web.component  -- 组件
- com.project.web.tags		-- 自定义标签/函数
- com.project.web.filter	-- 自定义过滤器
- com.project.web.aspect	-- AOP

* 更多与扩展
- com.project.web.xxx.controller -- 如果有更多的业务类型，可以使用扩展包

### project-middle

-- 项目中间件

* 构建中间件模块

[1]. 创建Simple Spring Maven项目: project-middle

[2]. pom.xml

```
<!-- 中间件模块：共用父模块版本号 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.project.middle</groupId>
<artifactId>project-middle</artifactId>
<packaging>jar</packaging>

<!-- 继承父模块 -->
<parent>
	<groupId>com.project.root</groupId>
	<artifactId>project-root</artifactId>
	<version>1.0.0</version>
</parent>
```

[3]. 职能说明

* 中间件负责管理第三方应用组件。
* 通常中间件已经配置完成，只需做少时的配置即可。
* 中间件模块的目的是确保第三方组件更易于配置和维护。

[4]. 常用的中间件列表

- 缓存服务 [memcached, redis]
- 消息服务中间件 [activeMQ, rabbitMQ, zeroMQ]
- 邮件系统 [Apache Commons Email]
- 短信系统
- 单点登录与会话设置
- 全文检索引擎 [solr, openSearch阿里云]
- 简单日志服务 [SLS阿里云]
- 文件系统 [OSS阿里云]