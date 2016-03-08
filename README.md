# 车智汇合作方应用接入说明

### 接入前提

1. 向车智汇申请一个appid（目前由人工指定）<br/>
    例如:2of7u147s7dEulAJkx9rPNtwsAc
2. 设置一个签名密钥salt（目前由人工指定），salt作为双方通讯验证对方的安全密钥，需要严格保密这个salt串<br/>
    例如:GIx8VA8x6RhFXwp

### 接入流程图
![Alt text](https://github.com/iauto360/open-api/blob/master/auth_flow.png)

### 接入流程说明
1. 用户在车智汇点击合作方的H5应用入口（应用模块或推广链接）。
2. 车智汇根据合作方的信息，给H5应用链接附加token参数。<br/>
     例如：http://xxx.sssss.com/a/b?token=405d1a6d598c7e27d33c8aff92efde76
3. 合作方H5应用服务端开始处理页面请求，从请求中获取token参数。
4. 拼接tokon、salt合成一个新字符串tokenSalt，采用sha1散列tokenSalt并使用16进制表示得到tokenSign。<br/>
    String tokenSalt = token + salt;     // tokenSalt = "405d1a6d598c7e27d33c8aff92efde76GIx8VA8x6RhFXwp" <br>
    String tokenSign = sha1Encode(tokenSalt);    // tokenSign = "22f10f84edefed2dbeb754cd10ffd610bb923611"
5. 采用post方式发送json格式的数据到平台接口https://open.iauto360.cn/api/auth/userinfo。 由于车智汇平台采用自行颁发的服务证书，请使用接口时忽略证书验证。<br/>
    `{"appid":"2of7u147s7dEulAJkx9rPNtwsAc","token":"405d1a6d598c7e27d33c8aff92efde76","tokenSign":"22f10f84edefed2dbeb754cd10ffd610bb923611","req":["mobile"]}`<br/>
     * appid:[接入前提1]获取
     * token:[接入流程3]获取到的token
     * tokenSign:[接入流程4]散列得到的tokenSign16进制字符串
     * req:表示需要请求的用户信息的数组，目前支持mobile。
6. 开放平台对合作方发送的token与tokenSign进行验证，确定是否从合作方发送。
7. 验证token通过后，开放平台返回用户信息到合作方服务端，其中code为0时表示正常返回。
    `{"uid":"dxBS0bR4GY6F2uoz8kqepkvkug4","mobile":"18666666666","time":1457436106549,"sugar":"zxy07)jB","code":0,"sign":"f630022518477f5ae78e1bd76c1351d1ff820947"}`<br/>
    * uid:用户在该合作方应用的唯一编号，同个用户从车智汇进入同个合作的应用的用户编号总保持都一致。
    * mobile:用户的手机号码
    * time:服务器返回时间，Unix时间戳，精确到毫秒
    * sugar:随机数，用于辅助验证数据是否由开放平台返回
    * code:执行结果，0为正常返回结果
    * sign:数据签名，用于验证数据是否由开放平台返回
8. 验证数据是否由开放平台返回。
    * 判断sign签名是否正确,拼接time、salt、sugar，采用sha1散列并使用16进制字符串表示，与sign进行对比，是否相同，如果相同，表示是由开放平台返回
    * 判断time与当前的时间差。由于sign，sugar，time可能被其它方拦截并恶意复用，强烈建议判断time与当前的时间是否相差过多<br/>
9. 处理合作方的自有业务逻辑，比如，新用户自动注册，设置用户登录状态，查询用户的业务信息等。
10. 合作方应用返回业务数据到车智汇内嵌浏览器。
11. 如果开放平台验证token不通过，返回错误信息回合作方服务端。
12. 与8同。
13. 合作方应用返回未登录的信息到车智汇内嵌浏览器。
14. 车智汇展示H5界面给用户。
  
    



