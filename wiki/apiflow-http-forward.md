# 可视化接口服务编排，HTTP转发

## 概述

基于 Vue 3 + TypeScript 开发的可视化接口服务编排组件，它提供了直观的拖拽式界面来设计和配置复杂的 API 服务流程。该组件特别专注于 HTTP 转发的可视化配置，让开发者能够通过图形化界面轻松构建微服务架构中的 API 网关和负载均衡器。

## 可视化接口服务编排

1、登录接口管理平台。

2、创建新接口，选用：接口编排 模式。  
![](/img/admin/apiflow/2.png)  

3、进行接口可视化编排，增加“HTTP转发”新节点  
![](/img/admin/apiflow/3.png)  

4、配置HTTP接口上游地址、接口参数和请求头等配置。
支持：IP哈希和按权重请求上游接口再种转发模式。  
![](/img/admin/apiflow/4.png)  

5、后台在线实时调试转发  
![](/img/admin/apiflow/5.png)  

6、一键发布新接口，设置接口图标、分配接口权限、填写接口描述等。  
![](/img/admin/apiflow/6.png)  

7、发布使用新接口  
![](/img/admin/apiflow/7.png)  

8、查看接口请求日志  
![](/img/admin/apiflow/8.png)  


## 核心特性

### 🎯 可视化流程编排
- **拖拽式设计**：支持从侧边栏拖拽节点到画布进行流程设计
- **节点连接**：通过连接点（Handle）实现节点间的数据流转
- **实时预览**：支持实时预览生成的代码和流程效果

### 🔄 HTTP 转发配置
- **负载均衡**：支持 WRR（加权轮询）和 IP Hash 两种负载均衡算法
- **多目标节点**：可配置多个后端服务节点，支持权重分配
- **请求头管理**：灵活配置 HTTP 请求头，支持常量、变量绑定和 Redis 缓存
- **请求体处理**：支持 JSON、Form-data、URL-encoded 等多种请求体格式
- **超时重试**：可配置请求超时时间和重试次数

### 🔧 流程控制
- **变量管理**：支持多种数据类型的变量定义和赋值

## 技术架构

### 核心技术栈
- **Vue 3**：使用 Composition API 构建响应式界面
- **TypeScript**：提供完整的类型定义和类型安全
- **Vue Flow**：基于 React Flow 的 Vue 实现，提供流程图核心功能
- **Element Plus**：UI 组件库，提供表单和交互组件

### 组件结构
```
YesFlowPane/
├── index.vue                 # 主组件入口
├── type.d.ts                 # TypeScript 类型定义
├── components/               # 子组件
│   ├── CustomNodes/          # 自定义节点组件
│   ├── NodeForms/            # 节点配置表单
│   ├── SideBar/              # 侧边栏
│   ├── NodeEditDrawer/       # 节点编辑抽屉
│   └── ...
├── utils/                    # 工具函数
│   ├── code-generate-engine.ts  # 代码生成引擎
│   ├── node-types.ts         # 节点类型定义
│   └── ...
└── hooks/                    # 组合式函数
```

## HTTP 转发功能详解

### 负载均衡算法

#### 1. WRR（加权轮询）
```typescript
type LoadingAlgorithmType = "wrr" | "ip_hash";
```
- **原理**：根据配置的权重值进行轮询分发
- **适用场景**：后端服务性能差异较大的情况
- **配置示例**：
  ```json
  {
    "destinationNodes": [
      {"url": "http://server1:8080", "weight": 3},
      {"url": "http://server2:8080", "weight": 1}
    ]
  }
  ```

#### 2. IP Hash
- **原理**：根据客户端 IP 地址的哈希值固定分发到特定服务器
- **适用场景**：需要保持会话一致性的场景

### 请求头配置

支持三种类型的请求头值：

```typescript
type VarValueType = "const_var" | "bind_var" | "redis_key";
```

1. **常量值（const_var）**：直接使用固定字符串
2. **变量绑定（bind_var）**：绑定流程中的变量值
3. **Redis 缓存（redis_key）**：从 Redis 中获取值

### 请求体处理

支持多种 Content-Type：

```typescript
type HttpRequestContentType = 
  | "none" 
  | "json" 
  | "form-data" 
  | "x-www-form-urlencoded" 
  | "raw";
```

### 代码生成示例

配置的 HTTP 转发节点会生成如下 Magic-Script 代码：

```javascript
// HTTP请求: 用户服务转发
var requestBody_2 = {
    "userId": userId,
    "action": "getProfile"
};
var requestBodyJson_2 = tool.objectToJson(requestBody_2);

var response_2 = http.request("http://user-service:8080/api/user/profile")
    .header("Authorization", "Bearer " + accessToken)
    .header("Content-Type", "application/json")
    .header("X-Request-ID", requestId)
    .body(requestBodyJson_2)
    .timeoutMS(5000)
    .retry(3)
    .post();

debug.print('HTTP请求结果: ' + response_2);

// 解析响应结果
var userProfile = tool.jsonToObject(response_2);
```

## 节点类型详解

### 流程控制节点

#### 1. 开始节点（Start）
- **功能**：定义 API 接口的输入参数
- **配置项**：参数名称、变量名、参数来源
- **参数来源**：requestParam、requestBody、requestHeader、accessTokenInfo

#### 2. 结束节点（End）
- **功能**：定义 API 接口的返回数据
- **配置项**：返回类型、输出变量、表达式

### 业务逻辑节点

#### 3. 变量节点（Var）
- **功能**：变量定义和赋值
- **支持类型**：string、int、long、float、double、boolean、list、map、null、expr

#### 4. . HTTP 请求节点（HttpRequest）
- **功能**：HTTP 转发和负载均衡
- **核心特性**：多目标节点、负载均衡、请求头管理、超时重试


## 使用示例

### 基础 HTTP 转发流程

1. **拖拽开始节点**：配置输入参数
2. **添加变量节点**：处理请求参数
3. **配置 HTTP 请求节点**：
   - 设置负载均衡算法
   - 配置目标服务器列表
   - 设置请求头和请求体
4. **添加结束节点**：配置返回数据


### 接口代码生成示例

```js
var str = requestParam ? requestParam.str : null; // 输入参数

/* 节点: 开始 开始 */

/* 节点: 打印变量 开始 */
debug.print('打印变量 str: '); // 打印变量
debug.print(str); // 打印变量

/* 节点: HTTP请求（转发接口） 开始 */
// 构建目标节点列表
var destinationList_3 = [
    {"api_url":"http://demo.phalapi.net/?s=App.Examples_Rule.Str","weight":1},
];
var selectedNode_3 = tool.loadWeight(destinationList_3);
var targetUrl_3 = selectedNode_3.api_url || "";
// 构建请求体
var requestBody_3 = {
    "str": str,
};
var requestBodyJson_3 = tool.objectToJson(requestBody_3);
// 构建curl请求
var response_3 = curl.create()
    .url(targetUrl_3)
    .body(requestBodyJson_3)
    .header("Content-Type", "application/json")
    .timeoutMS(6000) // 超时时间-毫秒 
    .retry(3) // 重试次数
    .post();

debug.print('HTTP请求结果: ' + response_3); // 打印响应

// 解析响应结果
var my_data = tool.jsonToObject(response_3);

/* 节点: 结束 开始 */
return {
    'my_data' : my_data
};
```

调试转发结果：
```json
{
    "code": 200,
    "message": "SUCCESS",
    "data": {
        "my_data": {
            "ret": 200,
            "data": "测试str abc",
            "msg": ""
        }
    },
    "debug": {
        "print": [
            "打印变量 str: ",
            "测试str abc",
            "HTTP请求结果: {\"ret\":200,\"data\":\"\\u6d4b\\u8bd5str abc\",\"msg\":\"\"}"
        ],
        "sql": null,
        "execution_time": "102毫秒"
    }
}
```

> 上游接口示例文档：http://demo.phalapi.net/docs.php?service=App.Examples_Rule.Str&detail=1&type=fold  


## 技术特点

### 1. 高度可扩展
- **自定义节点**：支持添加新的节点类型
- **插件化架构**：模块化设计，易于扩展功能
- **类型安全**：完整的 TypeScript 类型定义

### 2. 用户体验优化
- **拖拽操作**：直观的拖拽式界面设计
- **实时反馈**：操作过程中的实时视觉反馈
- **错误处理**：完善的错误提示和验证机制

### 3. 代码生成
- **Magic-Script 语法**：生成符合业务逻辑的脚本代码
- **模板化生成**：基于配置自动生成可执行代码
- **调试支持**：生成的代码包含调试信息

### 4. 数据持久化
- **JSON 导出**：支持流程配置的 JSON 格式导出
- **配置导入**：支持从 JSON 文件导入流程配置
- **版本管理**：支持流程配置的版本控制

## 最佳实践

### 1. 负载均衡配置
- 根据后端服务性能合理分配权重
- 考虑服务健康状态进行动态调整
- 配置合适的超时时间和重试策略

### 2. 错误处理
- 在流程中添加错误处理节点
- 配置合理的超时时间
- 实现降级策略

### 3. 性能优化
- 合理使用缓存机制
- 避免在循环中进行 HTTP 请求
- 优化数据库查询语句

## 总结

YesApi 可视化接口服务编排 组件通过可视化的方式简化了复杂的 API 服务编排工作，特别在 HTTP 转发配置方面提供了强大的功能。它让开发者能够：

- **快速构建**：通过拖拽方式快速构建 API 流程
- **灵活配置**：支持多种负载均衡算法和请求配置
- **易于维护**：可视化的流程设计便于理解和维护
- **高效开发**：自动生成代码，减少手动编码工作

该组件特别适用于微服务架构中的 API 网关、负载均衡器、服务编排等场景，为后端服务提供了强大的可视化配置能力。
