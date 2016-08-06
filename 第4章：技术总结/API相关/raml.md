# RAML

> RESTful API Modeling Language \(RAML\) makes it easy to manage the whole API lifecycle from design to sharing. It's concise - you only write what you need to define - and reusable. It is machine readable API design that is actually human friendly.

**整个 RAML 文档可简单划分为“版本声明”、“API 元数据定义”、“公用属性定义”和“资 源方法定义”四部分构成（参阅：**RAML规范解读**）**

官网：[http:\/\/raml.org\/](http://raml.org/)

apiworkbench: [http:\/\/apiworkbench.com\/docs\/](http://apiworkbench.com/docs/)

LocalAPI：[https:\/\/github.com\/isaacloud\/local-api](https://github.com/isaacloud/local-api)

faker：[https:\/\/github.com\/marak\/Faker.js\/](https://github.com/marak/Faker.js/)

RAML规范解读: [http:\/\/wenku.baidu.com\/link?url=CRBz9YUcStvGId76LyNQLFKEeDH9xrBiOjPKlo0J0TCAqZoycUa4C4uRjbJUFa3XkLoXFaS2H4GFvPYArstVkauAIAWuAG8ZYvIcS33pav7](http://wenku.baidu.com/link?url=CRBz9YUcStvGId76LyNQLFKEeDH9xrBiOjPKlo0J0TCAqZoycUa4C4uRjbJUFa3XkLoXFaS2H4GFvPYArstVkauAIAWuAG8ZYvIcS33pav7)

```
RAML相关的工具和资源在哪里?
官方网站：http://raml.org 
规范文档：http://raml.org/spec.html
设计器：https://github.com/mulesoft/api-designer
解析器：https://github.com/raml-org/raml-js-parser 

raml2html: https://github.com/raml2html/raml2html
raml2wiki: https://github.com/jhitchcock/raml2wiki
```

用RAML构建API文档: [http:\/\/mobilev5.github.io\/2016\/03\/06\/using-raml-build-api-doc\/](http://mobilev5.github.io/2016/03/06/using-raml-build-api-doc/)

介绍下 RAML: [https:\/\/testerhome.com\/topics\/4601](https://testerhome.com/topics/4601)

**[使用RAML描述API文档信息的一些用法整理：](http://www.cnblogs.com/darrenji/p/5198524.html)**http:\/\/www.cnblogs.com\/darrenji\/p\/5198524.html

## 步骤：

1. 安装[Atom](https://atom.io/)
2. OSX系统中，打开Atom编辑器，在preferences-install里搜索安装 api-workbench
3. 新建一个 RAML 项目
4. 安装 [nodeJS](https://nodejs.org/) ， `npm install -g localapi （可能需要翻墙）`
5. 启动 LocalAPI

> **$ **ls api.raml examples resourceTypes scripts traits documentation notebooks schemas securitySchemes
> 
> **$ **localapi run --no-examples api.raml
> 
> info: LocalAPI 1.5.0 by IsaaCloud.com
> 
> info: \[localapi\] Run app
> 
> info: \[localapi\] App running at http:\/\/localhost:3333

现在 GET [http:\/\/localhost:3333\/api\/v3\/nodes\/1](http://localhost:3333/api/v3/nodes/1) 就会返回 raml 文档定义的 responses 了

### 生成HTML文档

`安装raml2html： npm i -g raml2html 
 raml2html api.raml > api.html`

> IntoRobot raml 格式的开放API文档仓库： git clone [http:\/\/ram-lab.com\/molmc\_api.git](http://ram-lab.com/molmc_api.git)

### 名词释义

> 整个 RAML 文档可简单划分为“版本声明”、“API 元数据定义”、“公用属性定义”和“资 源方法定义”四部分构成

| 元素 | 说明 | 示例 |
| :--- | :--- | :--- |
| hello | hello | hello |
| hello | hello | hello |
| hello | hello | hello |
| hello | hello | hello |
| hello | hello | hello |

uriParameters:

formParameters:

queryParameters:

##### API元数据定义：

protocols：

mediaType:

documentation:

##### 元属性：

1. 
2. 

##### 重用属性定义：

一. 资源类型\(resourceType\)

> 定义资源时通过“type”属性定义所继承的资源类型,一个资源属于一个或零个资源类 型

二. 方法特征\(traits\)

> 定义资源时通过“is” 属性定义所使用的 trait

三. 结构模型\(schemas\)

> API 方法的入参或者返回结果如果为 json 或者 xsm 格式,通常需要“schema”属性来定 义该格式类型对应的结构。“schema”属性值既可以是结构模型的真实定义,也可以是之前 所定义结构模型的名称

四. 安全认证模型\(securitySchemes\)

> 假如一个 API 需要 OAuth 安全认证,这个 API 的所有资源和方法的定义必须包含“access\_token”查询参数。针对这种场景可以定义重用属性,让其他资源或方法通过继承的方式来获取属性的设置。

---

### 问题

1. 公用属性有哪些？ 如何使用？

  答： 公用属性有“Schemas”，“securitySchemas”, "ressourceTypes", "traits"等；通过重用属性的方法使用公用属性。

2. 如何写以下Json的schemas：

  ```
  { "data": [ { "score": "397", "city": "深圳市", "coordinate": [ 114.066112, 22.548515 ], "_id": "5582614a2239c27051000002", "dev_img": "assets/img/intodunio.png", "name": "智能风扇", "description": "控制风扇的开关" }, { "score": "534", "city": "北京市", "coordinate": [ 116.331398, 39.897445 ], "_id": "56475c672239c23d5d0000c4", "dev_img": "/v1/avatars/56475c672239c23d5d0000c4", "name": "智能垃圾桶", "description": "通过红外检测到有人体靠近需要丢垃圾，则通过舵机控制垃圾桶盖自动打开，丢完垃圾后垃圾桶盖自动关闭。" }, { "score": "324", "city": "衡阳市", "coordinate": [ 112.652199, 27.235328 ], "_id": "55db39942239c2395800002a", "dev_img": "/v1/avatars/55db47b62239c23a58000049", "name": "衡山空气监测", "description": "衡山PM2.5监控" } ], "msg": "Request processed successfully.", "code": 200}
  ```

  答：

  > {
  > 
  > "required": true,
  > 
  > "$schema": "[http:\/\/json-schema.org\/draft-03\/schema](http://json-schema.org/draft-03/schema)",
  > 
  > "type": "object",
  > 
  > "properties": {
  > 
  > "code": {
  > 
  > "type": "number",
  > 
  > "required": true
  > 
  > },
  > 
  > "msg": {
  > 
  > "required": true,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "data": {
  > 
  > "required": false,
  > 
  > "type": "array",
  > 
  > "items": \[
  > 
  > {
  > 
  > "type": "object",
  > 
  > "properties": {
  > 
  > "score": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "city": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "coordinate": {
  > 
  > "type": "array",
  > 
  > "required": false,
  > 
  > "items": {
  > 
  > "type": "number"
  > 
  > }
  > 
  > },
  > 
  > "\_id": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "dev\_img": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "name": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > },
  > 
  > "description": {
  > 
  > "required": false,
  > 
  > "type": "string"
  > 
  > }
  > 
  > }
  > 
  > }
  > 
  > \]
  > 
  > }
  > 
  > }
  > 
  > }


1. 

