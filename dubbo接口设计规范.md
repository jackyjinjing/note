
1 命名规范


1.1 dubbo接口统一命名为，XXXFacade，以区分内部Service


如：ProductFacade

1.2 获取一个对象以get开头
如：getById，getByAlias，getDetailById

1.3 获取多个对象以find开头
如：findByCondition，findByIds

1.4 获取分页对象以findPage开头
如：findPageByCondition

1.5 创建一个对象以create开头
如：createUser

1.6 更新一个对象以update开头
如：updateUser

1.7 删除一个对象以delete开头
如：deleteByAlias

1.8 如果有清晰的上下文，可以方法名中省略实体名称
如：ProductFacade.findProductByStatus，可以省略程ProductFacade.findByStatus

1.9 其他类型的操作使用具有业务含义的名称，以动词开头
如：ProductFacade.publish

2 出入参数规范
2.1 入参的第一个参数必须是kdtId
正例：getByAlias(Long kdtId, String alias)，findPage(Long kdtId, PageRequest request, Query query)

反例：getByAlias(String alias)，getByAlias(String alias, Long kdtId)

2.2 超过三个参数必须封装成请求对象
正例：findByCondition(Long kdtId, Query query);

反例：findByTypeAndStatusAndGroup(Long kdtId, Integer type, Integer Status, String group);



2.3 入参中，会对系统状态发生变更的操作，命名为XXXCommand，查询操作命名为XXXQuery

正例：findByCondition(Long kdtId, OrderQuery query)，update(ContentUpdateCommand command)

2.4 返回值命名为XXXDTO
正例：ContentDTO

2.5 dubbo接口禁止抛出异常
必须返回错误码和异常消息，不能抛出异常。

2.6 出入参需要实现序列化接口
没什么好说的，不实现会报错。

2.7 返回值必须继承BaseResult
推荐使用com.youzan.ebiz.common.model.CommonResult

2.8 分页请求必须使用com.youzan.ebiz.common.model.PageRequest
不带排序

{
	"pageNumber":1,
	"pageSize":20
}
带排序

{
	"pageNumber":1,
	"pageSize":20,
	"sort":{
		"orders":[
			{
				"direction":"ASC",
				"property":"createTime"
			}
		]
	}
}

2.9 分页返回值必须使用om.youzan.ebiz.common.model.Page
{
	"content":[
		1,
		2,
		3
	],
	"numberOfElements":3,
	"pageable":{
		"offset":0,
		"pageNumber":1,
		"pageSize":20
	},
	"total":100,
	"totalPages":5
}


2.10 禁止大而全的出入参，没有用的字段禁止传递
如：以下两个方法，返回值都是ContentBO，但是ContentBO中的某些详情字段，是只有getDetailByAlias才有的，尽量避免这种大而全的返回值，会使用者造成误解。

ContentBO getByAlias(Long kdtId, String alias);


ContentBO getDetailByAlias(Long kdtId, String alias);


2.11 出入参必须包含纯贫血对象，禁止包含方法


2.12 出入参有null语义的，禁止传递隐含默认值
2.13 C端前端请求通过ebiz-owl访问后端，B端前端请求通过owl-pc访问后端
2.14 应用分包按模块进行划分，然后模块内部在各自划分api、model、dto、service等包




