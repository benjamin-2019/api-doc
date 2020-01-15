# 参数加解密

## 参数加密

前端参数加密目前基于：`3DES` + `RSA公钥` 加密，可以通过添加地址栏参数：checkSign=true 来主动触发参数加密

所有参数KEY: usr 不变，参数值用：xiaoming - `3des('xiaoming', 3desKey)` 加密，

然后增加签名参数：`sign = rsa_pub(3desKey)`，用服务器提供的公钥， 对3desKey进行加密

```DEMO
请求路径：
?c=user&a=login

POST参数:  
usr=3des('xiaoming', 3desKey)
pwd=3des(md5('123456'), 3desKey)
sign=rsa_pub(3desKey) 

```

## 参数解密

服务器通过 `RSA私钥` 解出 `3desKey`，然后通过 `3desKey` 对每个参数进行解密，最终得到请求的数据

`注`：如果参数值是数组，则数组内的每个值需要再次遍历以及加密
