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

[4]. 包的命名约定[仅作参考]

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

[4]. 包的命名约定[仅作参考]

- com.project.utils.file -- 文件操作
- com.project.utils.lang -- 数据类型
- com.project.utils.http -- HTTP请求
- com.project.utils.text -- 文本操作
- com.project.utils.format -- 格式化

* 更多与扩展
- com.project.utils.xxx -- 以工具类的类型作为包的分类原则。

### project-model

-- 实体POJO模块

* 构建工具类模块

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

[4]. 包的命名约定[仅作参考]

- com.project.model -- 实体POJO。[注意：实体类的命名不要加相关的后缀名称]

* 更多与扩展
- com.project.model.xxx -- 如果有更多的业务类型，可以使用扩展包

* project-dao 		-- 数据访问层
* project-service 	-- 业务核心
* project-web 		-- PC端应用程序
* project-admin 	-- 后台管理程序
* project-app 		-- 移动客户端程序
* project-api 		-- OPEN API服务程序