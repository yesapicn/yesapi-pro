## 接口参数



### Get请求参数

```javascript
requestQuery.xxx //快捷获取query参数
request.getQuery("xxx") //获取单个query参数
request.getQuery()   //获取多个query参数
```



### Post body请求参数

```java
requestBody.xxx //快捷获取body参数
request.getBody("xxx") //获取单个body参数
request.getBody() //获取多个body参数
```



### Header请求参数

```javascript
requestHeader.xxx //快捷获取header参数
request.getHeader("xxx") //获取单个body参数
request.getHeader() //获取多个body参数
```



###  access_token解析出来的参数

```javascript
// 例子
{
	"app_key": "7Mx6NSG2SONveru2D3gbI4bu6", //应用key
	"user_level": 101, //用户等级
	"did": 121, //开发者ID
	"tid": 455  //tokenID
}


accessTokenInfo.app_key
```

