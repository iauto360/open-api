# 车智汇车联网开放数据服务
## 最新车况
获取终端当前绑定的车辆的最新的车况
### 接口
<ul>
<li>协议:HTTPS</li>
<li>服务号:1011</li>
<li>地址:https://open.iauto360.cn/srv/1011/{openId}/{验证签名}</li>
</ul>
### 请求参数
|字段名称|类型|必须|说明
|---|---|---|-----
|time|Int64|是|请求时间，采用unix时间，精确到毫秒
|data|Object|是|请求实际参数，内嵌json对象
|data.esn|String|是|终端编号
|plan|Int32|否|计费计划，不传该参数，将会自动匹配适用的计费计划
### 请求范例
``` json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167"
	},
	"plan":1008
}
```
### 返回数据
|字段名称|类型|必须|说明
|---|---|---|-----
|flag|Int32|	是|	请求接口的状态（ 1 成功 0 失败）
|data|Object|	是|	实际返回的业务数据，内嵌json对象
|data.faultCode|	Array|	否|	故障码，不存在该字段或数据为空表示当前无故障码
|data.location|	Array	|否|	最后上传坐标，采用GCJ-02坐标系：[经度,纬度]
|data.mileage|	Float|	否	|总里程，单位为公里
|data.voltage	|Float	|否	|最后上传电瓶电压
|err	|Int32|	否	|当flag为0时，返回的错误说明
### 返回范例
``` json
{
	"flag":1,
	"data":{
	  "faultCode":["P0003","P0010"],
		"location":[113.353982,23.149862],
		"mileage":1688.2,
		"voltage":13.2
	}
}
 ```
## 行车记录
获取终端当前绑定的车辆的行驶记录与和行驶的情况。
### 接口
<ul>
<li>协议:HTTPS</li>
<li>服务号:1012</li>
<li>地址:https://open.iauto360.cn/srv/1012/{openId}/{验证签名}</li>
</ul>

### 请求参数

|字段名称|类型|	必须|	说明
|---|---|---|-----
|time	|Int64|	是|	请求时间，采用unix时间，精确到毫秒
|data	|Object|	是|	请求实际参数，内嵌json对象
|data.esn	|String|	是	|终端编号
|data.timeFrom	|Int64	|是	|点火时间，范围查询开始时间，unix时间，精确到秒
|data.timeTo|	Int64	|是|	点火时间，范围查询结束时间，unix时间，精确到秒
|data.page|	Int32|	否|	页码，不传该参数，默认为1，每页数据为10条
|plan	|Int32	|否|	计费计划，不传该参数，将会自动匹配适用的计费计划

### 请求范例
``` json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167",
		"timeFrom":1483200000,
		"timeTo":1483372800,
		"page":2
	},
	"plan":1008
}
```
### 返回数据
|字段名称|	类型|	必须	|说明
|---|---|---|-----
|flag	|Int32|	是	|请求接口的状态（ 1 成功 0 失败）
|data	|Object|是|	实际返回的业务数据，内嵌json对象
|data.records	Array|	否|	行车记录数组
|data.records[].id|	Long|	是	|行车记录唯一ID
|data.records[].startTime|	Long	|是|	点火时间，unix时间，精确到秒
|data.records[].endTime|	Long|	是|	熄火时间，unix时间，精确到秒
|data.records[].mileage	|Float|	是|	里程，单位为公里
|data.records[].avgOil|	Float	|是|	平均百公里油耗，单位为升/百公里
|data.records[].totalOil|	Float|	是	|本次行车总油耗，单位为升
|data.records[].rapidAccelerate	|Int32|	否	|急踩油门次数
|data.records[].rapidBreak|	Int32|	否	|急踩刹车次数
|data.records[].hotTime	|Int32|	是|	热车时间，单位为秒
|data.records[].idleTime|	Int32	|是|	怠速时长，单位为秒
|data.records[].maxSpeed	|Float|	是	|最快速度，单位为公里/每小时
|data.totalCount	|Int32|	是|	总记录数
|page	|Int32|	是	|数据所在页码
|err|	Int32	|否	|当flag为0时，返回的错误说明
### 返回范例
``` json
{
	"flag":1,
	"data":{
		"records":[{
			"id":123332,
			"startTime":1483212344,
			"endTime":1483216462,
			"mileage":60.4,
			"avgOil":7.3,
			"totalOil":4.41,
			"rapidAccelerate":1,
			"rapidBreak":2,
			"hotTime":23,
			"idleTime":285,
			"maxSpeed":93.2
		}],
		"totalCount":11,
		"page":2
	}
}
```
## 行车轨迹
获取终端当前绑定的车辆的某行驶记录的完整轨迹。
### 接口
<ul>
<li>协议:HTTPS</li>
<li>服务号:1013</li>
<li>地址:https://open.iauto360.cn/srv/1013/{openId}/{验证签名}</li>
</ul>
### 请求参数
|字段名称|类型	|必须	|说明
|---|---|---|-----
|time	|Int64|	是	|请求时间，采用unix时间，精确到毫秒
|data	|Object	|是|	请求实际参数，内嵌json对象
|data.esn	|String|	是|	终端编号
|data.recordId	|Int64|	是	|行车记录唯一编号，由1012服务返回的Id
|plan|	Int32|	否	|计费计划，不传该参数，将会自动匹配适用的计费计划
### 请求范例
```json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167",
		"recordId":123332
	},
	"plan":1008
}
```
### 返回数据
|字段名称	|类型	|必须|	说明
|---|---|---|-----
|flag	|Int32	|是	|请求接口的状态（ 1 成功 0 失败）
|data|	Object|	是	|实际返回的业务数据，内嵌json对象
|data.coords	|Array	|否|	轨迹坐标数组，按时间先后排序
|data.coords[]	|Float|	是	|采用GCJ-02坐标系：[经度,纬度]
|err	|Int32	|否	|当flag为0时，返回的错误说明
### 返回范例
```json
{
	"flag":1,
	"data":{
		"coords":[
			[113.285831,23.145502],[113.289729,23.144023]
		]
	}
}	
```
## 提醒事件
获取终端当前绑定的车辆的提醒信息。
### 接口
<ul>
<li>协议:HTTPS</li>
<li>服务号:1014</li>
<li>地址:https://open.iauto360.cn/srv/1014/{openId}/{验证签名}</li>
</ul>
### 请求参数
|字段名称|类型|	必须|	说明
|---|---|---|-----
|time|	Int64|	是|	请求时间，采用unix时间，精确到毫秒
|data|	Object|	是	|请求实际参数，内嵌json对象
|data.esn	|String	|是	|终端编号
|data.timeFrom	|Int64|	是	|提醒时间，范围查询开始时间，unix时间，精确到秒
|data.timeTo|	Int64	|是|	提醒时间，范围查询结束时间，unix时间，精确到秒
|data.page|	Int32	|否|	页码，不传该参数，默认为1，每页数据为10条
|plan	|Int32|	否	|计费计划，不传该参数，将会自动匹配适用的计费计划
### 请求范例
``` json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167",
		"timeFrom":1483200000,
		"timeTo":1483372800,
		"page":2
	},
	"plan":1008
}
```
### 返回数据
|字段名称|	类型|	必须|	说明
|---|---|---|-----
|flag	|Int32|	是	|请求接口的状态（ 1 成功 0 失败）
|data|	Object|	是	|实际返回的业务数据，内嵌json对象
|data.records|	Array|	否|	提醒事件数组，按唯一编号从小到大排序
|data.records[].id|	Int64|	是|	提醒事件的唯一编号
|data.records[].esn	|String	|是|	发生提醒事件的终端编号
|data.records[].type|	String|	是	|提醒类型
|data.records[].time	|Int32|	是|	发生时间，unix时间，精确到秒
|data.records[].location|	Array	|否	|发生地点坐标，采用GCJ-02坐标系：[经度,纬度]
|data.records[].text	|String|	否|	提醒描述
|data.totalCount	|Int32	|是|	总记录数
|page	|Int32|	是	|数据所在页码
### 返回范例
``` json
{
	"flag":1,
	"data":{
		"records":[{
			"id":34567,
			"esn":"861677033025167",
			"type":"安防",
			"time":1468028564,
			"location":[113.353982,23.149862],
		},{
			"id":34567,
			"esn":"861677033025167",
			"type":"启动",
			"time":1468028564
		}],
		"totalCount":32,
		"page":4
	}
}
```
## 终端状况
获取终端的当前绑定信息。
### 接口
<ul>
<li>协议:HTTPS</li>
<li>服务号:1001</li>
<li>地址:https://open.iauto360.cn/srv/1001/{openId}/{验证签名}</li>
</ul>
### 请求参数
|字段名称|	类型|	必须	|说明
|---|---|---|-----
|time|	Int64|	是|	请求时间，采用unix时间，精确到毫秒
|data	|Object|	是|	请求实际参数，内嵌json对象
|data.esn|	String|	是	|终端编号
|plan	|Int32	|否	|计费计划，不传该参数，将会自动匹配适用的计费计划
### 请求范例
``` json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167"
	},
	"plan":1008
}
```
### 返回数据
|字段名称|	类型|	必须	|说明
|---|---|---|-----
|flag	|Int32|	是	|请求接口的状态（ 1 成功 0 失败）
|data	|Object	|是	|实际返回的业务数据，内嵌json对象
|data.bindStatus	|Int32	|是|	终端绑定状态：0-未绑定，1-已绑定
|data.mobile|	String|	否|	绑定的手机号码
|data.plateNo	|String|	否|	绑定的车牌号码
|data.inserted|	Int16	|否|	是否插入状态:0-拔出,1-插入
|data.activeTime	|Int64|	否	|最后数据时间,采用unix时间，精确到秒
|err	|Int32	|否	|当flag为0时，返回的错误说明
### 返回范例
``` json
{
	"flag":1,
	"data":{
		"bindStatus":1,
		"mobile":13820001828,
		"plateNo":"粤AD12345",
		"inserted":1,
		"activeTime":1468028564
	}
}
```
