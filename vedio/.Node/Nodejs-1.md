#  Node.js
    Node.js自身哲学，是花最小的硬件成本，追求更高的并发，更高的处理性能。
### 定义
    
    1、Node.js是一个让JavaScript运行在服务器端的开发平台，它让JavaScript的触角伸到了服务器端。
    2、Node.js不是一种独立的语言它使用JavaScript进行编程，运行在JavaScript引擎上（V8）。
    3、Node.js跳过了Apache、Naginx、IIS等HTTP服务器，它自己不用建设在任何服务器软件之上。
    
### 特点
    1、单线程
    
        单线程的带来的好处，还有操作系统完全不再有线程创建、销毁的时间开销。
    坏处，就是一个用户造成了线程的崩溃，整个服务都崩溃了，其他人也崩溃了。
    
    2、非阻塞I/O
    3、事件驱动
    
### 它适合做什么？
    当应用程序需要处理大量并发的I/O，而在向客户端发出响应之前，应用程序内部并不需要进行非常复杂的处理的时候
    Node.js非常适合。Node.js也非常适合与web socket配合，开发长连接的实时交互应用程序。

### Nodejs没有容器
    Node必须自己实现服务器,没有容器的概念，通过路由来解析url，并寻找实际页面，与iis Apache 有本质的区别。
```.js
var http  = require('http');
var fs =require('fs');

//构建服务器
var server = http.createServer(function(req,res){
        //执行的回调
        if(req.url=="/1"){
            fs.readFile('./test.html',function(err,data){
                res.writeHead(200,{"Content-type":"text/html;charset:UTF-8"});
                res.end(data);
            });
        }else{
            res.writeHead(404,{"Content-type":"text/html;charset:UTF-8"});
            res.end('404');
        }
});
//监听
server.listen(3000,"127.0.0.1");
```
### Nodejs Moudle

###### 1、Http
       
      使用http模块 
```.js
//引入Http模块
var http  = require('http');
//引入url模块
var url = require('url');
//构建服务器/req为客户端请求对象，res为返回对象
var server = http.createServer(function(req,res){
    //客户端请求的url
   var reqUrl= req.url;
   //获取请求的Get参数
   var param = url.parse(req.url,true).query;
   //写入客户端的请求头
   res.writeHead(200,{});
   //返回客户端数据
   res.end(data);
});

//服务开始监听
server.listen(3000,"127.0.0.1");
```

###### 1、fs
     读取文件流的函数
     
``` .js
//利用立即执行函数解除异步(迭代器)
fs.readdir('./1',function(err,files){
   var fileArray = [];
   (function iterator(i){
       if(i==files.length){
           console.log(fileArray);
           return;
     }

         fs.stat("./1/" + files[i],function(err,stats){
	    //检测成功之后做的事情
            if(stats.isDirectory()){
                //如果是文件夹，那么放入数组。不是，什么也不做。
                fileArray.push(files[i]);
            }
            iterator(i+1);
        });
   
   })(0);
		
 });
```
### 总结

    1、Node.js开发服务器，数据、路由
    2、Node.js实际上是极客开发出的一个小玩具,不是银弹,有着别人不具备的怪异特点:单线程、Non-blocking I/O、Event Driven
    
##### tip
	    首先，Node不为每个用户开辟一个线程，所以非常极端的选择了单线程。单线程
	要照顾所有的用户，那么就必须有非阻塞I/O，否则一个人的I/O就把别人、自己都阻塞了。
	一旦有非阻塞I/O，一个人如果I/O去了，就会放弃CPU的使用权，换成另一个人使用CPU（或者执行此人后面的语句）
	所以CPU的利用率100%。
	    第一个人I/O结束了,就要用事件来通知线程，执行回调函数。此时必须有事件环，就有一个排队调度机制。
	Node中有超过半数的C++代码，在搭建事件环。
	
###### Node.js和别的老牌3P不一样
	没有自己的语法，使用V8引擎，所以就是JS。V8引擎解析JS的，效率非常高，并且V8中很多东西都是异步的。
	Node就是将V8中的一些功能自己没	有重写（别人做了，自己就站在巨人肩膀上），移植到了服务器上。
        没有web容器，就是安装配置完成之后，没有一个根目录。
