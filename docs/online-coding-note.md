## 注意事项

### 避免与内置变量冲突
在线开发中，有系统内置的变量供开发者使用，开发者在命名变量时应该避免与其重名，防止被覆盖

系统内置变量 | 说明 
:--------|:--------
onlineApiController|请求的控制器参数
onlineApiMethod|请求的方法参数
requestBody|请求的body参数
requestHeader|请求的header参数
requestParam|GET请求的url参数
accessTokenInfo|accessToken解析后的参数
db|数据库操作模块的对象
curl|Curl模块的对象
tool|内置工具类
debug|调试工具类
request|请求对象，用来获取请求参数
ip|客户端请求IP



### 关于Oracle数据库的注意事项

在使用Oracle数据库，表名和字段名尽量统一用大写（不要用双引号创建表名和字段），否则涉及到表名和字段名，需要用双引号，这无疑增加了代码开发的繁琐程度

```javascript
// 正常大写的写法
db.connect("db128oracle").table("user").fetchAll();

//如果表名用的是双引号的写法
db.connect("db128oracle").table("\"user\"").fetchAll();
```

