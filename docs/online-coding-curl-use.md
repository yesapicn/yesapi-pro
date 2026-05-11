## Curl使用

Curl是一个轻量级、易用的HTTP客户端库，它提供了简洁的链式API，支持同步请求，以及各种HTTP方法（GET、POST、PUT、DELETE等）。



### 基础用法



#### GET请求

```javascript
var response = curl.create()
    .url("https://api.example.com/users")
		.query({"p1":111,"p2":222})
    .get();
```



#### POST请求

```javascript
var jsonBody = "{\"name\":\"John\",\"age\":30}";
var response = curl.create()
    .url("https://api.example.com/users")
    .header("Content-Type", "application/json")
    .body(jsonBody)
    .post();
```



#### 其他HTTP请求

```javascript
// PUT请求
var response = curl.create()
    .url("https://api.example.com/users/1")
    .body(jsonBody)
    .put();

// DELETE请求
var response = curl.create()
    .url("https://api.example.com/users/1")
    .delete();

// PATCH请求
var response = curl.create()
    .url("https://api.example.com/users/1")
    .body(jsonBody)
    .patch();
```



### 高级用法



#### 设置请求头

```javascript
var response = curl.create()
    .url("https://api.example.com/users")
    .header("Authorization", "Bearer token123")
    .header("Accept", "application/json")
    .get();
或
var response = curl.create()
    .url("https://api.example.com/users")
    .header({"Authorization":"Bearer token123","Accept":"application/json"})
    .get();
```



#### 设置请求体

```javascript
var jsonBody = "{\"name\":\"John\",\"age\":30}";
var response = curl.create()
    .url("https://api.example.com/users")
    .header("Content-Type", "application/json")
    .body(jsonBody)
    .post();
```



#### 设置超时

```javascript
var response = curl.create()
    .url("https://api.example.com/users")
    .timeout(30)//单位秒
    .get();
或
var response = curl.create()
    .url("https://api.example.com/users")
		.timeoutMS(100) //单位毫秒
    .get();
```



#### 设置重试次数

```javascript
var response = curl.create()
  .url("https://api.example.com/unreliable-endpoint")
  .retry(3)  // 最多重试3次
  .retryDelay(2)  // 每次重试间隔2秒
  .timeout(5)  // 每次请求超时5秒
  .get();
```

