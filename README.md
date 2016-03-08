# 车智汇合作方应用接入说明

### 接入前提

* 从车智汇申请一个appid（目前由人工指定）<br/>
    例如:2of7u147s7dEulAJkx9rPNtwsAc
* 设置一个签名密钥salt（目前由人工指定），salt作为双方通讯验证对方的安全密钥，需要严格保密这个salt串<br/>
    例如:GIx8VA8x6RhFXwp

### 接入流程图
![Alt text](https://github.com/iauto360/open-api/blob/master/auth_flow.png)

### 接入流程说明
1. 用户在车智汇点击合作方的H5应用入口（应用模块或推广链接）
2. 车智汇根据合作方的信息，给H5应用链接附加token参数<br/>
     例如：http://xxx.sssss.com/a/b?token=405d1a6d598c7e27d33c8aff92efde76
3. 合作方H5应用服务端开始处理页面请求，从请求中获取token参数
4. 拼接token与salt合成一个新字符串tokenSalt，采用sha1散列tokenSalt并使用16字制得到tokenSign<br/>
    String tokenSalt = token + salt;     // tokenSalt = "405d1a6d598c7e27d33c8aff92efde76GIx8VA8x6RhFXwp" <br>
    String tokenSign = sha1Encode(tokenSalt);    // tokenSign = "22f10f84edefed2dbeb754cd10ffd610bb923611"
5. 采用post方式发送json格式的数据到平台接口https://open.iauto360.cn/api/auth/userinfo<br/>
    `{"appid":"2of7u147s7dEulAJkx9rPNtwsAc","token":"405d1a6d598c7e27d33c8aff92efde76","tokenSign":"22f10f84edefed2dbeb754cd10ffd610bb923611","req":["mobile"]}`<br/>
其中，req表示需要请求的数据，目前只支持mobile
6. 开放平台对合作方发送的token与tokenSign进行验证，确定是否从合作方发送
7. token验证通过返回用户信息到合作方服务端，其中code为0
    



