# 模块
    
    在Node.js中，以模块为单位划分所有功能,并且提供了一个完整的模块加载机制,这时我们可以将应用程序划分为各个不同的部分
    
    Node.js中，一个JavaScript文件中定义的变量、函数，都只在这个文件内部有效。当需要从此JS文件外部引用这些变量、函数时
    必须使用exports对象进行暴露。使用者要用require()命令引用这个JS文件。
    
### 一、模块导出语法
###### `1、使用方式`
`test这个变量,是一个js文件内部才有作用域的变量,如果别人想用这个变量，那么就要用exports进行暴露。`
```.js
//foo.js
//定义变量
var test="exportMoudle";
//导出变量
exports.test = test;


var target = require('./foo.js');
console.log(target.test);
```

`描述一个类，用module.export = ’构造函数名‘的方式向外暴露一个类。`
```.js
function Ctr(){
}
module.export = Ctr;
```
###### `2、js文件和js文件之间有两种合作的模式`
    1>某一个js文件中，提供了函数，供别人使用。只需要暴露函数就行了； exports.test=test;
    2>某一个js文件，描述了一个类。   module.exports = People;

### 二、引入模块

###### `1、通常情况下引入需要加对应文件的相对路径，如果不想加入文件目录路径那么必须用到”node_modules“文件夹`

    例如：var foo = require("foo.js");   //没有写./l路径,所以不是一个相对路径。是一个特殊的路径。
    那么Node.js将该文件视为node_modules目录下的一个文件,.node_modules文件夹并不一定在同级目录里面
    在任何直接祖先级目录中，都可以。也可以放到NODE_PATH环境变量的文件夹中
    
    1、首先建立node_modules文件夹并加入自己的模块js
    2、那么在调用方直接可以使用文件名来调用
    
###### `2、require()中的路径，是从当前这个js文件出发，找到目标。而fs是从命令提示符找到目标`

    1、如果读取文件就需要"__dirname+文件名"
    
``` .js
//桌面上有一个a.js， test文件夹中有b.js、c.js、1.txt
//a要引用b：
var b = require(“./test/b.js”);
//b要引用c：
var b = require(“./c.js”);
//fs等其他的模块用到路径的时候，都是相对于cmd命令光标所在位置
fs.readFile(__dirname + "/1.txt",function(err,data){
	if(err) { throw err; }
	console.log(data.toString());
});
```
    
###### `3、使用文件夹来管理模块`

    1、每一个模块文件夹中，推荐都写一个package.json文件，这个文件的名字不能改。
    2、node将自动读取里面的配置。有一个main项，就是入口文件：默认为index.js
    3、如果自定义配置了main入口名称，那么系统将会读取定义的文件
```.js
var bar = require("bar");
//那么Node.js将会去寻找node_modules目录下的bar文件夹中的index.js去执行
//在package.json中将main改为app.js
{
  "name": "kaoladebar",
  "version": "1.0.1",
  "main" : "app.js"
}

```
       
######  `4、引用NPM`
    1、初始化package.json ---npm init
    2、安装依赖 ---npm install
  ```.json
   "dependencies": {
    "silly-datetime": "^0.1.2" --- 固定版本如果三个都加标记那就固定为当前版本
  },
  ```
       
### 三、客户端请求

###### `1、Post`
	相比较GET请求，POST请求比较复杂。因为Node.js认为，使用POST请求时，数据量会比较多。
    为了追求极致的效率，它将数据拆分成为了众多小的数据块(chunk)，然后通过特定的事件，将这些小数据块有序传递给回调函数。
    
```.js
var http = require('http');
var querystring = require("querystring");

http.createServer(function (req, res) {
    var postData = "";
    //分段接收post参数
    req.addListener('data', function (chunk) {
        postData += chunk;
    });
    //最终完成的字符串
    req.addListener('end', function () {
        var datastring = postData.toString();
        res.end("success");
        //将datastring转为一个对象
        var dataObj = querystring.parse(datastring);
    })

}).listen(8080, "127.0.0.1");
```

###### `2、Get`

    获取请求的Get参数
    var param = url.parse(req.url,true).query;

###### `3、使用NPM上的formidable实现文件传输`
```.js
var util = require('util'),
    fb = require('formidable');

var form = new fb.IncomingForm();
//设置上传路径
form.uploadDir = './uploads';
//回调中获取值
form.parse(req, function (err, fields, files) {
    console.log(fields);
    console.log(files);
    res.writeHead(200, { 'content-type': 'text/plain' });
    res.write('received upload:\n\n');
    //传入文件必须在表单中加enctype="multipart/form-data"
    res.end(
        '<form action="/upload" enctype="multipart/form-data" method="post">' +
        '<input type="text" name="title"><br>' +
        '<input type="file" name="upload" multiple="multiple"><br>' +
        '<input type="submit" value="Upload">' +
        '</form>'
    );
});
```

### 四、模板引擎

