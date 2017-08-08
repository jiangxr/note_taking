### jeesite使用总结  
**<font color="red">jeesite是什么？</font>**  
JeeSite 是一个企业信息化开发基础平台，Java企业应用开源框架，Java EE（J2EE）快速开发框架，使用经典技术组合（Spring、Spring MVC、Apache Shiro、MyBatis、Bootstrap UI），包括核心模块如：组织机构、角色用户、权限授权、数据权限、内容管理、工作流等。

**<font color="red">内置功能</font>**
1. 用户管理：用户是系统操作者，该功能主要完成系统用户配置。
2. 机构管理：配置系统组织机构（公司、部门、小组），树结构展现，可随意调整上下级。
3. 区域管理：系统城市区域模型，如：国家、省市、地市、区县的维护。
4. 菜单管理：配置系统菜单，操作权限，按钮权限标识等。
5. 角色管理：角色菜单权限分配、设置角色按机构进行数据范围权限划分。
6. 字典管理：对系统中经常使用的一些较为固定的数据进行维护，如：是否、男女、类别、级别等。
7. 操作日志：系统正常操作日志记录和查询；系统异常信息日志记录和查询。
8. 连接池监视：监视当期系统数据库连接池状态，可进行分析SQL找出系统性能瓶颈。
9. 工作流引擎：实现业务工单流转、在线流程设计器。

**<font color="red">技术选型</font>**
1. 后端

  - 核心框架：Spring Framework 4.1
  - 安全框架：Apache Shiro 1.2
  - 视图框架：Spring MVC 4.1
  - 服务端验证：Hibernate Validator 5.2
  - 布局框架：SiteMesh 2.4
  - 工作流引擎：Activiti 5.21
  - 任务调度：Spring Task 4.1
  - 持久层框架：MyBatis 3.2
  - 数据库连接池：Alibaba Druid 1.0
  - 缓存框架：Ehcache 2.6、Redis
  - 日志管理：SLF4J 1.7、Log4j
  - 工具类：Apache Commons、Jackson 2.2、Xstream 1.4、Dozer 5.3、POI 3.9

2. 前端

  - JS框架：jQuery 1.9。
  - CSS框架：Twitter Bootstrap 2.3.1
  - 客户端验证：JQuery Validation Plugin 1.11。
  - 富文本在线编辑：CKEditor
  - 在线文件管理：CKFinder
  - 动态页签：Jerichotab
  - 手机端框架：Jingle
  - 数据表格：jqGrid
  - 对话框：jQuery jBox
  - 下拉选择框：jQuery Select2
  - 树结构控件：jQuery zTree
  - 日期控件： My97DatePicker

**<font color="red">安全考虑</font>**

  1. 开发语言：系统采用Java 语言开发，具有卓越的通用性、高效性、平台移植性和安全性。
  2. 分层设计：（数据库层，数据访问层，业务逻辑层，展示层）层次清楚，低耦合，各层必须通过接口才能接入并进行参数校验（如：在展示层不可直接操作数据库），保证数据操作的安全。
  3. 双重验证：用户表单提交双验证：包括服务器端验证及客户端验证，防止用户通过浏览器恶意修改（如不可写文本域、隐藏变量篡改、上传非法文件等），跳过客户端验证操作数据库。
  4. 安全编码：用户表单提交所有数据，在服务器端都进行安全编码，防止用户提交非法脚本及SQL注入获取敏感数据等，确保数据安全。
  5. 密码加密：登录用户密码进行SHA1散列加密，此加密方法是不可逆的。保证密文泄露后的安全问题。
  6. 强制访问：系统对所有管理端链接都进行用户身份权限验证，防止用户直接填写url进行访问。
<hr>  

<font color="green">嗯。。上面部分主要来源于github的jeesite的自己的说明，下面主要针对本人的使用情况进行一些说明。</font>

感觉jeesite中用到的技术内容还是挺杂的，在使用的过程中并没有都很多细节进行深究，主要根据jeesite实现自己的一些需求。比如jeesite提供了很多的安全验证内容，这块内容就没有进行深究。

目前使用jeesite主要用到了如下几个功能：
1. 菜单管理  
菜单管理功能还是不错的，在系统设置的菜单管理中，能够对菜单项进行动态的添加和删除，并且能够给不同的人员分配不同的菜单权限。

2. 代码生成
之前没有接触过类似的代码生成的功能，但是使用起来感觉还是不错的，生成的代码严格按照分层的格式，在生成代码之前，首先得创建表，需要在表的基础上才能生成相应的代码。
拿请假表举例：
```SQL
CREATE TABLE `askforleave` (
  `id` varchar(100) NOT NULL,
  `applicant` varchar(50) DEFAULT NULL COMMENT '请假人',
  `leavetype` varchar(100) DEFAULT NULL COMMENT '请假类型',
  `reason` varchar(100) DEFAULT NULL COMMENT '请假原因',
  `begintime` datetime DEFAULT NULL COMMENT '开始时间',
  `endtime` datetime DEFAULT NULL COMMENT '结束时间',
  `ismanager` varchar(100) DEFAULT NULL COMMENT '是否是经理',
  `proc_ins_id` varchar(64) DEFAULT NULL COMMENT '流程实例ID',
  `comment` varchar(200) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='请假流程测试表';
```
根据以上的表可以生成如下7个文件：
1. AskforleaveDao.java //数据访问层的代码
2. Askforleave.java //实体类的代码
3. AskforleaveService.java //服务层的代码，调用dao层，实现一些接口供controller层调用
4. AskforleaveController.java //控制层的代码
5. AskforleaveDao.xml //dao层使用mybatis框架，此为dao层mybatis的配置文件
6. askforleaveForm.jsp //表单页面，可编辑
7. askforleaveList.jsp //列表页面，显示表中的所有数据

上面主要是根据代码生成器生成的部分代码，当然了，这些代码还是比较简单的，主要是实现了增删改查的功能。根据需要在此基础上进行修改。 目前来看，需要了解mybatis的使用，比如存在外键，多表关联等，通过代码生成可能完成不了类似的功能，这些东西需要进行一定的配置。

3. 业务流程管理
这个功能，很多人使用这个软件时可能并不关注，目前，我使用这个软件主要就是想使用业务流程管理的功能，单从目前来看，作者在这个模块中的开发并不完善，作者主要提供了，流程的在线编辑，和一些简单的流程管理功能。如果仅仅想简单实现一些流程的配置，目前来看，jeesite能够实现，只需要调用jeesite提供的一些接口，但是这web端并没有提供很好的操作界面。并且针对流程的一些复杂操作，jeesite并没有提供，主要需要我们自己根据activiti来实现一些接口。比如会签操作等。

下面根据一个简单的请假流程操作来进行一定的说明。
1. 在线办公-》模型管理-》新建模型
![请假流程](/images/12.png)

2. 进行模型的配置，比如开始节点的配置
![模型配置](/images/13.png)

3. 部署流程
![部署流程](/images/14.png)

4. 在线办公-》我的任务-》新建任务
此时就可以根据部署的流程新建任务，通过流程管理，能够方便的进行人员的控制，并能够轻松的跟踪流程的运行状态。

5. 流程的部分是比较好弄的，比较复杂是根据代码来实现流程的管理。比如前加签、后加签和会签等。如果想实现这些复杂的功能，需要进行activiti的api的学习，然后在项目中实现相应的接口。
