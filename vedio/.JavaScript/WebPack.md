# WebPack
          Webpack具有Grunt、Gulp对于静态资源自动化构建的能力，但更重要的是，Webpack弥补了requireJS在模块化方面的缺陷
    同时兼容AMD与CMD的模块加载规范，具有更强大的JS模块化的功能。

#### 一、JS规范

###### 1.CommonJS
       采用同步的require方法来加载依赖并返回导出的接口。一个模块可以通过往exports对象上添加属性或者设置module.exports的
    值来确定导出哪些接口。
    
- ##### `优点`

      1.服务端的模块可以被复用
      2.已经有很多模块供使用了（npm）
      3.很容易上手使用

- ##### `缺点`

      1.阻塞式的调用不能适用于网络请求，因为网络请求是异步的
      2.无法同步require多个模块

- ##### `使用方法`

```.js
    require("module");
    require("../file.js");
    exports.doStuff = function(){};
    module.exports = someValue;

```

###### 1.AMD

    异步require。
    
- ##### `优点`

      1.能满足网络请求的异步需求
      2.能同步加载多个模块


- ##### `缺点`

      1.代码复杂，更难写也更难读
      2.似乎是一种绕路的笨办法


- ##### `使用方法`

```.js
    require(["module","../file.js"],function(module,file){/*...*/});
    define("mymodule",["dep1","dep2"],function(d1,d2){
        return someExportValue;
    });

```

###### 1.CMD
    在 CMD 规范中，一个模块就是一个文件，每个 API 都简单纯粹，职责单一
- ##### `使用方法`

```.js

//define 接受 factory 参数，factory 可以是一个函数，也可以是一个对象或字符串。
//factory 为对象、字符串时，表示模块的接口就是该对象、字符串。比如可以如下定义一个 JSON 数据模块：
//factory 为函数时，表示是模块的构造方法。执行该构造方法，可以得到模块向外提供的接口。
//factory 方法在执行时，默认会传入三个参数：require、exports 和 module
    
define(function(require,exports,module){
    //模块代码
});


```
