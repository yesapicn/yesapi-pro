## 客户端SDK

YesAPI接口平台的SDK，下载和示例使用。主要用于：设置接口域名、申请获取令牌Token、以及接口请求调用和返回结果。  

基础能力：  
 + 设置请求头及```access-token```令牌；  
 + 设置GET方式的Query请求参数；
 + 设置POST方式的Body请求体参数；
 + 请求平台API接口；

高级支持：  
 + 接口请求参数加密（可选）；
 + 接口返回结果数据解密（可选）；

伪代码：  
```
// 初始化接口客户端
object client = new YesAPISDK({
    'host': 'http://java.demo.yesapi.cn/' // 设置接口域名
});

// 申请获取令牌
string token = client.AuthApplyToken(应用app_key, 应用密钥); // 应用app_key + 密钥

// GET请求接口，返回结果
object result = client.header({
    'access-token': token
}).get('/api/test/print_hello_world?请求参数=xxxxx', 超时时间);

// POST请求接口，返回结果
object result = client.header({
    'access-token': token
}).post('/api/test/print_hello_world', 请求体参数，超时时间);

// 针对POST请求体参数的加密请求

// 初始化接口客户端，更多高级配置
object client = new YesAPISDK({
    'host': 'http://java.demo.yesapi.cn/', // 设置接口域名
    'aesKey': 32dWoR8HEPIiwdju', // AES密钥
    'enableRequestBodyEncrypt': true, // 开启请求参数加密，默认关闭
    'enableResponeDataDecrypt': true, // 开启返回数据解密，默认关闭
});
```

### curl

通过```access-token```请求头设置访问令牌，指定请求接口路径后，使用GET、POST或其他方式进行接口请求，返回获得JSON结果。  

例如，成功请求：  
```bash
curl -X POST "http://java.demo.api.yesapi.cn/api/test/print_hello_world" \
-H "Content-Type: application/json" \
-H 'Accept: application/json, text/plain, */*' \
-H "access-token: 接口访问令牌" \
--data-raw '{}'
```
成功返回：  
```json
{"code":200,"message":"SUCCESS","data":{"hello_world":"hello world"}}
```

失败请求（例如令牌缺失或错误）：
```bash
curl -X POST "http://java.demo.api.yesapi.cn/api/test/print_hello_world" \
-H "Content-Type: application/json" \
-H 'Accept: application/json, text/plain, */*' \
-H "access-token: xxxxxxx" \
--data-raw '{}'
```
失败返回：  
```json
{"code":406,"message":"access-token校验不通过","data":null}
```

其他API接口请求用法与之类似，获取令牌的接口是：```/api/official/auth/apply_token``` 申请访问令牌。

### Java SDK

<a href="sdk/JAVA.zip?raw=true" download>下载 JAVA SDK</a>  

用法说明
引入依赖：
```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.25</version>
</dependency>
```

Java调用示例：
```java
//实例调用
YesapiClient client = new YesapiClient(
    "http://java.api.xxx.cn",  // API域名
    "xxx",             // 加密密钥
    "xxx",                 // 应用key
    "xxx",              // 应用密钥
    true                            // 自动初始化token
 );

// POST参数
Map<String, Object> postData = new HashMap<>();
postData.put("b_string", "aaa");
postData.put("b_int", 123);
postData.put("b_float", 1.123);
postData.put("b_bool", true);
postData.put("b_array", "1,2,3");

// GET参数
Map<String,String> paramsData = new HashMap<>();
paramsData.put("p_string", "han");
paramsData.put("p_int", "789");
paramsData.put("p_float", "7.789");
paramsData.put("p_bool","false");
paramsData.put("p_array", "7,8,9");

// 发起接口请求
response = client
    .path("online-api/mai/test/params")        // 设置请求路径
    .body(postData)                 // 设置POST数据
    .params(paramsData)
    .enableParamEncrypt(true)       // 启用参数加密
    .enableResponseDecrypt(true)    // 启用响应解密
    .retry(3)                       // 设置重试次数
    .timeout(60)                    // 设置超时时间（秒）
    .headers(new HashMap<String, String>() {{  // 设置自定义请求头
        put("h-string", "header");
    }})
    .post();  
```

### Python SDK

<a href="sdk/Python.zip?raw=true" download>下载 Python SDK</a>  

Python使用示例：  
```python
# 配置信息
API_URL = "https://api.example.com" # 设置请求路径
APP_KEY = "your-app-key" # 应用key
APP_SECRET = "your-secret-key" # 应用密钥
AES_KEY = "your-aes-key" # 加密密钥

# 请求示例
def response(guess1=None, guess2=None, guess3=None, decrypt: bool = False):
    result_post = callApi(
        route='/online-api/v1/post_sdk/test', # 设置请求路径
        method='POST', # 设置请求方式
        data={'guess1': guess1, 'guess2': guess2, 'guess3': guess3}, # 设置请求参数
        decrypt=decrypt, # 响应解密
        timeout=10, # 请求超时时间（秒）
        retry_times=3, # 请求失败自动重试次数
        retry_wait=1 # 请求失败自动重试等待时间（秒）
    )
    print(result_post)
    return result_post
```

### Golang SDK

<a href="sdk/Golang.zip?raw=true" download>下载 Golang SDK</a>  

Go请求示例：  
```Go
// 配置信息
const (
	API_URL    = "https://api.example.com" // 设置请求路径
	APP_KEY    = "your-app-key" // 应用key
	APP_SECRET = "your-secret-key" // 应用密钥
	AES_KEY    = "your-aes-key" // 加密密钥
)

// 请求示例
func response(guess1, guess2, guess3 interface{}, decrypt bool) (map[string]interface{}, error) {
	data := map[string]interface{}{} // 设置请求参数
	if guess1 != nil {
		data["guess1"] = guess1
	}
	if guess2 != nil {
		data["guess2"] = guess2
	}
	if guess3 != nil {
		data["guess3"] = guess3
	}

	result, err := CallAPI(APIRequest{
		Route:  "/online-api/v1/post_sdk/test", // 设置请求路径
		Data:   data,
		Method: "POST", //设置请求方式
	}, decrypt, 10*time.Second, 3, 1*time.Second) //响应解密, 请求超时时间（秒）, 请求失败自动重试次数, 请求失败自动重试等待时间（秒）
	if err != nil {
		return nil, fmt.Errorf("调用POST接口失败: %w", err)
	}

	return result, nil
}
```

### PHP SDK

<a href="sdk/PHP.zip?raw=true" download>下载 PHP SDK</a>  

PHP用法说明：  
```php
<?php
require_once './YesapiClient.php';
$yesapiClient = new YesapiClient(
    'http://java.xxx.cn',  // API域名
    'xxx',         // 加密密钥
    'xxx',             // 应用key
    'xxx',          // 应用密钥
    true                        // 自动初始化token（默认为true）
);
$result = $yesapiClient->path('online-api/mai/test/params')
  ->body(['b_string'=>'mai','b_int'=>123,'b_float'=>1.123,'b_bool'=>true,'b_array'=>'1,2,3'])-params(['p_string'=>'han','p_int'=>789,'p_float'=>7.789,'p_bool'=>false,'p_array'=>'7,8,9'])
  ->headers(['h-string'=>'headers'])
  ->enableParamEncrypt(true)
  ->enableResponseDecrypt(true)
  ->post();
```

### Nodejs SDK

<a href="sdk/Nodejs.zip?raw=true" download>下载 Nodejs SDK</a>  

NodeJs用法说明
```javascript
// 配置信息
const API_URL = "https://api.example.com"; // 设置请求路径
const APP_KEY = "your-app-key"; // 应用key
const APP_SECRET = "your-secret-key"; // 应用密钥
const AES_KEY = "your-aes-key"; // 加密密钥

//调用示例
async function response({ guess1, guess2, guess3 } = {}, decrypt = false) {
    try {
        const result = await callApi('/online-api/v1/post_sdk/test', { // 设置请求路径
            method: 'POST', // 设置请求方式
            data: { guess1, guess2, guess3 }, //参数
            decrypt, // 响应解密
            timeout: 10000, // 设置超时时间（毫秒）
            retryTimes: 3, // 设置重试次数
            retryWait: 1000 // 设置请求失败自动重试等待时间（毫秒）
        });
        console.log(result);
        return result;
    } catch (err) {
        console.error(err.message);
    }
}
```

### C-Sharp SDK

<a href="sdk/C-Sharp.zip?raw=true" download>下载 C-Sharp SDK</a>  

C-Sharp用法说明：
```c#
// 创建YesapiClient实例（自动初始化token）
YesapiClient client = new YesapiClient(
  "https://your-api-domain.com",  // API域名
  "your-encrypt-key",             // 加密密钥
  "your-app-key",                 // 应用key
  "your-app-secret",              // 应用密钥
  true                            // 自动初始化token
);
var postData = new Dictionary<string, object>
{
  { "name", "John Doe" },
  { "email", "john@example.com" }
};

var response2 = await client
  .Path("api/user/create")        // 设置请求路径
  .Body(postData)                 // 设置POST数据
  .EnableParamEncrypt(true)       // 启用参数加密
  .EnableResponseDecrypt(true)    // 启用响应解密
  .Retry(3)                       // 设置重试次数
  .Timeout(60)                    // 设置超时时间（秒）
  .Headers(new Dictionary<string, string> {  // 设置自定义请求头
    { "X-Custom-Header", "value" }
  })
  .Post();                   // 执行POST请求
```

