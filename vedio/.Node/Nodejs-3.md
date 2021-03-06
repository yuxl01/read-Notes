# Express

  Express框架是后台的Node框架
  
### 一、Express框架
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
### 二、路由

###### 1、访问类型

 `url不区分大小写，利用正则，冒号动态参数使用路由`
  
``` .js
const express = require('express');
const app = new express();
//get请求
app.get("/aAb",function(req,res){
	res.send('Hello World');
});
//post请求
app.post("abc",function(req,res){
	
});
//支持任何method的请求
app.all("/",function(){
	
});

//利用冒号动态参数
app.get('/student/:id/:name', function (req, res) {
   var id = req.params["id"];
   var name =req.params["name"]
});

 RESTful路由设计一个路径，但是http method不同，对这个页面的使用也不同
```
### 三、中间件

	1、如果在express中存在相同路由的并且get、post回调函数中没有next参数，那么就匹配上第一个路由，就不会往下匹配了,只有调用next才会继续往下匹配

``` .js
const express = require('express');
const app = new express();

app.get("/:user/:id", function (req, res) {

    console.log('1');
    res.send("用户信息" + req.params.user);
	//只有使用了next,当前没匹配上才会往下
    next();
});

app.get("/admin/login", function (req, res) {

    console.log('2');
    res.send("admin is logined");
});


app.listen(3000);
```

	2、app.use()是一个特殊的中间件，第一个参数不写“/”就代表所有的路径。

```.js
app.use("/admin", function (req, res) {
		//输出所有 admin/aa/bb/cc/dd
         res.write(req.originalUrl + "\n");  
		//输出第一级 admin
         res.write(req.baseUrl + "\n");  
		//输出子级 aa/bb/cc/dd
        res.write(req.path + "\n");      
    res.end("Hello World");
});

```