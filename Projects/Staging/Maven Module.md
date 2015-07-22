# Maven 多模块项目构建

## 模块说明

_说明：本文使用Spring Tool Suite作为开发工具_

### project-root / project-parent

-- 顶级父类模块，命名规则：使用root或parent。

* 构建父模块

[1.] 创建Simple Spring Maven项目: project-root

[2.] pom.xml
```
  <!-- 父模块 -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.project.root</groupId>
  <artifactId>project-root</artifactId>
  <version>1.0.0</version>
  <!-- packing的非常重要，父模块必须是pom，其他模块可使用jar或war，方便打包 -->
  <packaging>pom</packaging>
  
  <!-- 关联子模块：项目构建完成后让父模块关联所有子模块 -->
  <modules>
  	<module></module>
  </modules>
```

* project-root 		-- 顶级父类模块
* project-utils 	-- 通用工具模块
* project-model 	-- 实体POJO
* project-dao 		-- 数据访问层
* project-service 	-- 业务核心
* project-web 		-- PC端应用程序
* project-admin 	-- 后台管理程序
* project-app 		-- 移动客户端程序
* project-api 		-- OPEN API服务程序