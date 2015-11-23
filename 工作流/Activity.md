涉及的表
-------
* act_ge_bytearray 二进制数据表  部署文件信息表
* act_re_deployment  

* RE 	存储
* RU	运行时表
* HI	历史数据
* ID	组织机构
* GE	全局通用数据及设置

主要对象
-------
	ProcessEngine创建 
	repositoryService（持久化服务）
	runtimeService（运行时服务）
	formService（表单服务）
	identityService（身份信息）
	taskService（任务服务）
	historyService（历史信息）
	managementService（管理定时任务）

* 流程引擎      		工厂,是流程框架的工厂对象
* 持久化服务对象		RepositoryService 与数据库相关的服务对象,主要进行部署,以及流程定义的查询相关操作
* 运行时服务对象		RuntimeService		与运行相关的服务对象,主要进行流程启动操作
* 任务服务对象		TaskService			与任务相关的服务对象,主要进行历史流程相关操作
* 历史服务对象		HistoryService		与历史信息相关的服务对象,主要进行历史流程实例相关操作
* 流程部署			Deployment			部署信息,是记录部署流程文件的时间等信息的对象
* 流程定义			ProcessDefinition	流程定义,用于记录一次部署文件的主题信息,一次部署,对应一个流程定义
* 流程实例			ProcessInstance		流程实例,记录启动进入流程的流程定义相关信息,每启动一次流程定义,对应一个流程实例对象
* 任务				Task 				记录流程中每个任务节点信息,一个流程实例,对应若干任务
* 历史流程定义		HistricProcessInstance	记录一个流程实例的历史信息,每个历史流程

流程
----
* 1,获取流程引擎
	ProcessEngine processEngine = ctx.getBean("processEngine", ProcessEngine.class);
* 3, 获取持久化服务对象
	RespositoryService repositioryService = processEngine.getRepositoryService();
* 4, 部署流程文件,保存<创建部署文件,增加类路径,部署>
	 Deployment deploy = repositoryService.createDeployment()
							.addClasspathResource("MyProcess.bpmn").deploy();
	Sysout.out.println(deploy);

* 创建流程定义查询对象
	ProcessDefinitionQuery query = repositoryServiceDeffintionQuery();
	* 查询list 		List<processDefinition> pds = query.list();
	* 查询单一数据	query.latestVersion().singleResult();
	* 排序查询       query.orderByProcessDefinitionVersion().desc().list();
	* 分页查询		query.listPage(1,2);
	* 查询数据条数	query.count();

* 启动流程实例
	* 涉及到的表
		act_hi_actinst  : 流程实例历史节点表 流程实例的节点历史信息,代表流程实例的执行的步骤
		act_hi_procinst : 流程实例历史信息表 流程实例的历史相关信息.
		act_hi_taskinst : 历史任务信息 任务的历史信息
		act_ru_execution: 当流程实例结束,表中对应信息删除,运行的流程实例执行信息表
		act_ru_task		: 运行的任务信息表
	1,查询要启动的定义
	RuntimeService runtimeService = processEngine.getRuntimeService();
	runtimeService.startProcessInstanceById();


* 网关 判断
	1 排他网关(决策)
	2 并行网关(会签) 没有判断能力,无分支判断,要求所有的分支必须全部审批通过,才能结束流程
	3 包含网关