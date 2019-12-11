# Express

  Express框架是后台的Node框架
  
### Express框架
    原生Node开发，会发现有很多问题。比如：
    
  	  ■ 呈递静态页面很不方便，需要处理每个HTTP请求，还要考虑304问题
	  ■ 路由处理代码不直观清晰，需要写很多正则表达式和字符串函数
	  ■ 不能集中精力写业务，要考虑很多其他的东西
    
###### 1、使用Express
    
```.js
//NPM安装Express
//--save参数，表示自动修改package.json文件，自动添加依赖项。
npm install--save express
//引入模块
var express = require("express");
var app = express();
app.get('/', function (req, res) {
    res.send('Hello World');
});
//正则表达式路由
app.get(/^\/index\/([\d]{10})$/, function (req, res) {
    res.send("页面" + req.params[0]);
});

app.listen(3000);
```
 ###### 2、使用静态服务
 
 ```.js
var express = require('express');
var app = new express();
//开启静态服务
app.use(express,static('./public'));
 ```
 
 ###### 3、调用模板引擎
    1、创建index.ejs html页
 ``` .html
 <!DOCTYPE html>
<html lang="en">

<head>
    <title>Document</title>
</head>
<body>
    <h1>使用express测试模板页</h1>
    <ul>
        <% for(var i=0;i<news.length;i++){%>
        <li><%=news[i]%></li>
        <%}%>
    </ul>
</body>
</html>
 ```
    2、绑定数据源
 ```.js
const express = require('express');
const app = new express();

//设置模板引擎
app.set("view engine", 'ejs');
//创建数据源
app.get('/', function (req, res) {
    res.render('index', {
        "news": ["1", "2", "3"]
    });
})
app.listen(3000);
 ```
### 路由

### 中间件
