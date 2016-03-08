# 车智汇合作方应用接入说明

### 接入前提

* 从车智汇申请一个appid，例如:2of7u147s7dEulAJkx9rPNtwsAc
* 设置一个签名密钥salt，例如:GIx8VA8x6RhFXwp

### 接入流程图
![Alt text](https://github.com/iauto360/open-api/blob/master/auth_flow.png)

### 接入流程说明
1. 用户在车智汇点击合作方的H5应用入口（应用模块或推广链接）
2. 车智汇根据合作方的信息，给H5应用链接附加token参数
3. 合作方H5应用处理从车智汇打开的网页，从请求中获取token参数
4. 拼接token与salt合成一个新字符串tokenSalt，采用sha1散列tokenSalt并使用16字制得到tokenSign
5. 
    



