## 内置模块 

内置模块为一些常用的工具类模块，在脚本解析时会自动加载，开发者可以直接在代码中调用。



### tool - 工具模块

#### json字符串转换成jsonObject对象

```javascript
var curlData = curl.create()
    .url("http://pro-demo.yesapi.cn/api/app.php?s=App.HelloWorld.Say")
    .get();
var result = tool.jsonToObject(curlData);

return result;
```



#### 将map或者list对象转json字符串

```javascript
var list = [
  {"api_url":"aaa.com","weight":10},
  {"api_url":"bbb.com","weight":20},
  {"api_url":"ccc.com","weight":30},
  {"api_url":"ddd.com","weight":40},
  {"api_url":"fff.com","weight":50},
];
var tojson = tool.objectToJson(list);
```





#### 哈希sha256加密

```javascript
var hash = tool.hashEncrypt("192.168.0.1");
```



#### 随机字符串

```javascript
var randomStr = tool.randomStr(10); 
```



#### md5加密

```javascript
var md5Str = tool.md5("Hello");
```



#### 十六进制转十进制

```javascript
var hexToInt = tool.hexToInt("7E9");
```



#### 字符串截取

```javascript
//格式 tool.substr(被截取的字符串,起始位,截取位数)
var strsub  = tool.substr("你好，世界！Hello世界123",0,5);//从0开始截取，截取5位
```



#### 字符串替换

```javascript
//格式 tool.strReplace(被替换的字符串,旧字符,新字符)
var strReplace = tool.strReplace("你好，世界！Hello世界123","世界","Mai");
```



####  生成UUID

```javascript
var uuid = tool.uuid();
```



#### 去掉字符串前后的空格

```javascript
var strTrim  = tool.trim(" 你好 ");
```



#### 当前日期时间

````javascript
var now  = tool.now();
````



#### 将字符串按分隔符切分成数组

```javascript
//格式 tool.explode(被分割的字符串,分割标识符);
var explode = tool.explode("111,222,333",",");
```



#### 将数组合并成字符串

```javascript
//格式 tool.implode(戴合并的List,合并标识符);
var implode = tool.implode(["aaa","bbb","ccc"],"-");//输出：aaa-bbb-ccc
```



#### 将数组合并成带双引号并且用英文逗号隔开的字符串

```javascript
var implodeIn = tool.implodeIn(["aaa","bbb","ccc"]); //输出："aaa","bbb","ccc"
```







### debug - 调试模块

#### 打印输出

```javascript
debug.print('打印字符串');
debug.print(123);
debug.print(1.2345);
debug.print(true);
debug.print([1,2,3]);
debug.print({name:"mai",age:30});

//返回结果例子
{
    "code": 200,
    "message": "SUCCESS",
    "data": {
    	...   
    },
    "debug": {
        "print": [
            "打印字符串",
            "123",
            "1.2345",
            "true",
            "[1, 2, 3]",
            "{name=mai, age=30, info={avatar=111.jpg, nickname=麦}}"
        ],
        "sql": null,
        "execution_time": "6毫秒"
    }
}
```

