# 车智汇车联网开放数据服务接入说明

## 服务接入方式
由数据服务接入方采用HTTPS协议调用车智汇Web API主动获取数据，详细请阅接口服务调用。

## 接口服务调用
### 服务地址

https://open.iauto360.cn/srv/{服务号}/{openId}/{验证签名}

|字段名称|说明
|----|----------
|服务号|用户调用数据查询的服务代号+
|openId|对外开放唯一Id
|验证签名|用于双方信息安全签名验证
### 调用方式
采用HTTPS协议POST方式访问
### 参数说明
参数采用JSON格式数据，通过POST方式请求接口地址
|字段名称|必须|说明
|----|---|-------
|time|是|请求时间，采用unix时间，精确到毫秒
|data|是|请求实际参数，内嵌json对象，根据不同服务有不同的内容
|plan|否|计费计划，不传该参数，将会自动匹配适用的计费计划
### 参数例子
``` json
{
	"time":1468024618564,
	"data":{
		"esn":"861677033025167"
	},
	"plan":1008
}
```
## 签名生成
### 生成步骤
<ol>
<li>拼接openId、请求参数、安全码、请求时间（该时间必须与请求参数里的time一致）</li>
<li>采用md5散列算法散列刚才拼接出来的字符串</li>
<li>转为全小写</li>
</ol>
### 伪代码
``` java
String openId = "your openid";
String secureCode = "your securecode";
Long time = now();
String param = JSON({"time":1477484314,......});
String signStr = md5(openId + param + secureCdoe + time);
```
## 接口返回
采用JSON格式的字符串返回查询结果
### 返回内容说明
|字段名称|说明
|----|----------
|flag|请求接口的状态（ 1 成功 0 失败）
|data|实际返回的业务数据，内嵌json对象
|err|当flag为0时，返回的错误说明
### 返回例子
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
