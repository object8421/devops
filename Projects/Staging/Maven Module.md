# Maven 多模块项目构建

## 模块说明

=说明：本文使用Spring Tool Suite作为开发工具=

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

com.project.root.model  			-- 基础，通用的Model
com.project.root.dao 				-- 数据访问层通用接口
com.project.root.dao.impl  			-- 数据访问层通用实现类
com.project.root.service   			-- 服务层通用接口
com.project.root.service.impl   	-- 服务层通用实现类
com.project.root.controller   		-- 控制层通用类
com.project.root.annotation 		-- 自定义注解
com.project.root.interceptor 		-- 自定义拦截器 [配合注解使用]
com.project.root.aspect				-- AOP [使用AOP处理相关业务]
com.project.root.component  		-- 业务组件 [系统业务组件]
com.project.root.factory    		-- 工厂类 [负责核心业务调度]
...									-- 更多
com.project.root.xxx.dao			-- 项目中涉及多种业务模块，可以定义扩展包

* project-root 		-- 顶级父类模块
* project-utils 	-- 通用工具模块
* project-model 	-- 实体POJO
* project-dao 		-- 数据访问层
* project-service 	-- 业务核心
* project-web 		-- PC端应用程序
* project-admin 	-- 后台管理程序
* project-app 		-- 移动客户端程序
* project-api 		-- OPEN API服务程序