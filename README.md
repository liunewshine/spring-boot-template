#  [Spring Boot Template(项目模板)](https://github.com/liunewshine/spring-boot-template.git) 


### 配合使用的`vue`前端项目和截图，请移步[vue-admin-template](https://github.com/liunewshine/vue-admin-template.git)


### 项目演示：[地址](http://118.25.44.86:8011) 账号密码默认填充（admin，a123456）

### 若文档不能正常查看，请移步 [文档地址](https://www.yuque.com/docs/share/e7e73f84-692c-41d2-9e29-3e3a06c3daa9)

## 简介


多个`spring boot前后端分离管理系统`开发经验总结而成，使用`restful`风格`API`，
包含`角色` `权限` `代码生成` `增删改查示例`，
集成`mybatis plus`无需书写`sql`即可实现大部分接口。


项目功能完整，结构特别简单易懂，未做多余封装，上手难度极地。可作为模板项目使用，大大提高开发效率。


特别适合`中小型项目`、`新手`和`个人开发者`参考及使用，一天开发一个项目不是梦。


## 开发环境


- **JDK 1.8 +**
- **Maven 3.5 +**
- **IntelliJ IDEA ULTIMATE 或者 Eclipse** (_安装 _`_lombok_`_ 插件_)
- **Mysql 5.7 +**(_mysql8 驱动名称和之前不一样，项目示例为mysql8驱动名称_)



## 技术选型
| 名称 | 介绍 |
| --- | --- |
| Spring Boot 2.2.1.RELEASE | 无 |
| [mybatis-plus 3.2.0](https://github.com/baomidou/mybatis-plus) | 用着超爽的`mybatis`
插件，可在`baseService`
中实现常用的数据库操作 |
| [jwt-token](https://github.com/auth0/java-jwt) | 使用`jwt`
生成`token`
，实现登录认证 |
| [swagger 2.9.2](https://github.com/swagger-api/swagger-ui) | 生成项目文档，供app和前端使用 |



## 工程结构


```
template-api
├── template-generator -- 代码生成器
│   └── Generator.java -- 运行此java生成代码
└── template-main -- 业务模块
    ├── base包 -- 常用父类
    │   ├── BaseEntity.java -- 包含一个id，所有Entity的父类
    │   ├── BaseLogEntity.java -- 继承BaseEntity，包含createBy,createTime,updateBy.updateTime四个日志相关的field
    │   ├── BaseService.java -- 继承自mybatis plus，包含常用的增删改查方法，具体使用参考mybatis plus文档
    │   └── BaseSvo.java -- svo为search vo简写，用于接收前端传递查询条件，包含pageNum和pageSize
    ├── config包 -- 项目配置
    │   ├── @Auth.java -- 此注解用于实现权限验证，添加到@Auth("permission")到controller方法鉴权
    │   ├── @NoAuth.java -- 默认所有的controller方法需要登录，使用这个注解排除登录，例如登录注册接口
    │   ├── MybatisPlusConfig.java -- mybatis plus配置文件，分页在此配置
    │   ├── MyMetaObjectHandler.java -- mybatis plus自动填充功能，可以自动插入createBy,createTime,updateBy.updateTime值
    │   ├── AuthAspect.java -- aop实现鉴权，和访问日志
    │   ├── MvcConfig.java -- spring mvc常用配置，包含跨域，时间格式化等等
    │   ├── ProjectParam.java -- 此项目的配置文件映射，自动读取application.properties中的私有配置
    │   └── Swagger2.java -- Swagger2配置文件
    ├── controller包 -- 使用restfull风格api
    │   ├── AccountController.java -- 账户相关，登录、修改密码等
    │   ├── DeptController.java -- 部门，树形结构的参考此模块，自动转换为前端通用数据结构
    │   ├── DeviceController.java -- 增删改查示例模块
    │   ├── PermissionController.java -- 权限
    │   ├── RoleController.java -- 角色
    │   ├── UserController.java -- 用户
    │   └── 角色 权限 用户模块因为 多对多中间表 问题，写了好多sql，其他模块使用通用方法基本都能解决
    ├── entity包 -- 和数据库对应，继承BaseEntity.java或者BaseLogEntity.java
    ├── exception包 -- 异常类
    │   ├── MyException.java -- 常见的异常，例如参数校验，是否存在等抛出此异常，前端httpstatus自动为400
    │   ├── NoAuthException.java -- 未登录访问接口抛出此异常，前端httpstatus自动为401
    │   └── UnauthorizedException.java -- 登录但是无具体权限抛出此异常，前端httpstatus自动为403
    ├── mapper包 -- mybatis mapper
    ├── service包 -- 服务类，继承BaseService.java
    ├── svo包 -- svo为search vo简写，用于接收前端传递查询条件，继承BaseSvo.java
    ├── util包 -- 工具类，包含BCrypt密码加密、树形结构转换、jwt token生成，日期工具，获取当前用户工具，json工具等
    └── vo包 -- 相当于dto包，集成字entity，用于传递entity中不包含的参数给前端
```


## 运行项目


1. clone或者下载项目
2. 使用`Intellij idea`或者`Eclipse`导入`maven`项目
3. 创建`mysql`数据库，执行`项目根目录/sql/db.sql`
4. 修改`application-dev.properties`中的数据库连接参数
5. 运行`template-main`中的`TemplateApplication.java`即可启动项目 **(请运行前端项目查看实际界面效果)**
[vue-admin-template](https://github.com/liunewshine/vue-admin-template.git)
6. 更改名称，由于是模板项目，所有的`项目名称`、`包名`、`artifactId`、`modules`、`Dockerfile`、
`代码生成器`等均为`template`，建议按照自己的项目名称修改，也可以不改。



## 部署项目


#### 使用docker部署


```
#进入项目目录
cd template-api

#打包(可在本地打包，提交jar后，到服务器运行docker命令）
mvn install

#进入Dockerfile所在文件夹
cd template-main

#docker打包镜像
docker build -t template-api:latest .

#启动docker容器,可使用-e SPRING_PROFILES_ACTIVE=${env}指定环境，不指定则默认为dev
docker run -itd --name template-api -e SPRING_PROFILES_ACTIVE=dev template-api:latest
```


#### 若您的docker服务器开放了远程访问（外网记得启用TLS,否则不安全），可直接使用idea推送至服务器


配置参考


![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314873276-6f38cb54-e02a-4466-ac87-360772afbfe8.png#clientId=u73c290d9-a3c4-4&from=paste&id=uec8d5c8a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1333&originWidth=1920&originalType=url&ratio=1&size=452100&status=done&style=none&taskId=u88006a68-2960-4eba-b14e-35700cd762e)


#### 使用docker+jenkins部署


参考我的文章
[最优雅的Docker+Jenkins pipeline部署Spring boot项目](https://juejin.im/post/5d906c40e51d45780e4ce9d2)


#### 直接部署


```
#进入项目目录
cd template-api

#打包
mvn install

#linux 在jar所在目录执行
nohup java -jar template-api.jar --spring.profiles.active=dev >log.txt &

#windows 在jar所在目录执行
java -jar template-api.jar --spring.profiles.active=dev
```


## mybatis-plus使用


特别强大的mybatis工具，封装了大量通用方法，写很少的sql即可完成一个项目。


- 自动填充通用字段
- 逻辑删除
- 多租户(saas服务)
- 分页
- 等等



详细文档[MyBatis-Plus文档](https://mp.baomidou.com/guide/)


## 代码生成器使用


代码生成器由[mybatis-plus](https://github.com/baomidou/mybatis-plus)提供，详情请参考
[mybatis-plus 代码生成器](https://mp.baomidou.com/guide/generator.html#%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)


1. 修改com.step.generator.Generator.java中的配置参数
- ![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314895087-03108c37-67f4-47b0-a97b-91901d2e1aa8.png#clientId=u73c290d9-a3c4-4&from=paste&id=ue91d855e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=987&originWidth=2657&originalType=url&ratio=1&size=145135&status=done&style=none&taskId=u78b04315-3ed8-4058-9221-7edee45e34a)
2. 执行main函数
3. 控制台输入表名

- ![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314905259-f258ce4b-d894-43f4-b5bb-ab0ee25eafe1.png#clientId=u73c290d9-a3c4-4&from=paste&id=u1093ed9e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=294&originWidth=750&originalType=url&ratio=1&size=19577&status=done&style=none&taskId=u80522c59-ca41-474c-8731-06c9a5a0a1f)



## swagger2文档使用


文档配置文件`com.step.template.main.config.Swagger2`


只有`dev`和`test`环境会生成文档，可以修改配置文件的@Profile注解自定义


文档默认访问地址`http://localhost:8020/swagger-ui.html`


未添加`@NoAuth`的api需要设置token后才可访问
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314926947-1b18a2a3-e5ee-4cb3-bdf3-dc283cc6875b.png#clientId=u73c290d9-a3c4-4&from=paste&id=uffd3ff97&margin=%5Bobject%20Object%5D&name=image.png&originHeight=876&originWidth=1802&originalType=url&ratio=1&size=89358&status=done&style=none&taskId=u90dfa76f-ba6d-40c1-a201-887f7567b39)


## 权限注解使用方法


- `@NoAuth` -- 默认所有的`controller`方法需要登录，使用这个注解排除登录，例如登录注册接口
- `@Auth` -- 此注解用于实现权限验证，添加到`@Auth("permissionName")`到`controller方法中`鉴权
- `permissionName`建议权限值采用`user:query,user:edit`类似的形式，参考数据库中的`permission`表
- `com.step.template.main.config.AuthAspect`中会拦截所有`controller`方法，根据注解进行相应的处理



![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314937485-a5652193-bfb3-4492-8081-303e975e489c.png#clientId=u73c290d9-a3c4-4&from=paste&id=ub99edfcd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=961&originWidth=2056&originalType=url&ratio=1&size=155334&status=done&style=none&taskId=u86d65765-6357-475c-9351-1719da285a8)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314942380-65afcddf-608e-4d9c-9e2f-2064cd219499.png#clientId=u73c290d9-a3c4-4&from=paste&id=u8c34abbf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=544&originWidth=1056&originalType=url&ratio=1&size=69753&status=done&style=none&taskId=u90b2a7a0-5465-4024-b0a1-78753bff792)


## 权限管理系统设计思路


#### 我们先说一种权限管理最简单的做法


- 既然权限管理系统，必须有`用户表`和`权限表`
- 一个`用户`可以有多个`权限`，一个`权限`可以分配给多个`用户`，所以是`多对多`关系。
- `多对多`会产生关联表，表示如下：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314953095-b6bdfb35-3780-431b-bbd9-7a14f4cbb6fe.png#clientId=u73c290d9-a3c4-4&from=paste&id=u12a360be&margin=%5Bobject%20Object%5D&name=image.png&originHeight=432&originWidth=575&originalType=url&ratio=1&size=25586&status=done&style=none&taskId=u8e125899-4612-4e20-acff-ccd8724b8e0)


这种设计已经可以满足小型管理系统的需求，使用和开发都特别简单。
但是如果用户数增大，运营人员需要为每个用户分配权限，会浪费大量的精力，而且容易出错。
用户多，但是用户的`角色`比如`经理`、`运维`等却很少，所以应该增加一层角色。


#### RBAC基于角色的权限访问控制(Role-Based Access Control)


取消了`用户`和`权限`的直接关联，改为通过`用户关联角色`、`角色关联权限`的方法来间接地赋予用户权限。
当新增用户时只需分配已有的角色，大大减少运营人员工作量和出错率。


-  三张表分别为多对多关系
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314962935-202a0f43-8f3a-45a1-87cd-2bdeddb9c18a.png#clientId=u73c290d9-a3c4-4&from=paste&id=u4d04b711&margin=%5Bobject%20Object%5D&name=image.png&originHeight=114&originWidth=900&originalType=url&ratio=1&size=17854&status=done&style=none&taskId=ue75d1b8b-1686-47af-aabe-72cb5c50ffd) 
-  生成关联网之后的结构如下
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314967341-d5ec452b-97f0-46e9-8d6b-a807ce1cfb90.png#clientId=u73c290d9-a3c4-4&from=paste&id=ua0eed0d0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=993&originWidth=908&originalType=url&ratio=1&size=88964&status=done&style=none&taskId=u8fc112cc-fb54-45ac-890a-8f5c5264df9) 



## 提示


- 仔细查看[MyBatis-Plus文档](https://mp.baomidou.com/guide/)
- 仔细查看`工程结构`中项目结构介绍
- 除了查询列表(往往带有分页和模糊查询)自己写sql以外，其他情况尽量使用BaseService中的通用方法。
单个查询的使用多个service中的通用方法组合，而不是写sql，单个数据不会有性能问题。
- 数据库表和字段一定要添加comment，代码生成会根据comment生成swagger2文档
- Controller中接口返回的json数据未做包装，直接返回数据。
- 本项目根据http status判断是否成功，200等2开头的状态为成功，400,500等为失败。
前端ajax，axios会自动走到catch方法，处理特别简单。
- 如需外层包装成功失败状态，建议在aop或者拦截器中实现。



```
#本项目成功时返回结果如下
{
  "name": "姓名",
  "deptName": "部门名称"
}
#失败时
{
  "name": "姓名",
  "deptName": "部门名称"
}
#而不是
{
  "error": 0,
  "msg": "success",
  "data": {
    "name": "姓名",
    "deptName": "部门名称"
  }
}
```


-  `com.step.template.main.util.ScopeUtil`中可获取当前用户 
-  使用构造器注入，避免在`private`字段上写`@Autowired`，这么写idea会提示警告
`Field injection is not recommended`。构造器注入配合lombok写法很简洁。
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314978222-01418dbc-5530-4483-a888-780e1c69f4a3.png#clientId=u73c290d9-a3c4-4&from=paste&id=uf0ad52ba&margin=%5Bobject%20Object%5D&name=image.png&originHeight=477&originWidth=1107&originalType=url&ratio=1&size=39251&status=done&style=none&taskId=u9d806465-40a9-453e-9df1-225fcf211ec) 
-  `BaseService`提供字段唯一性检测，参考`userService`里的`phone`唯一检测
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12878014/1626314983601-743f92a4-0dd7-4452-9b90-523034060478.png#clientId=u73c290d9-a3c4-4&from=paste&id=ucf01fb54&margin=%5Bobject%20Object%5D&name=image.png&originHeight=948&originWidth=1809&originalType=url&ratio=1&size=119955&status=done&style=none&taskId=u79fc51d8-c0ec-4e49-9710-876981342b6) 



## 欢迎start，有疑问、建议、bug请提issue
