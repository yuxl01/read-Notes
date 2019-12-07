# 模块
    
    在Node.js中，以模块为单位划分所有功能,并且提供了一个完整的模块加载机制,这时我们可以将应用程序划分为各个不同的部分
    
    Node.js中，一个JavaScript文件中定义的变量、函数，都只在这个文件内部有效。当需要从此JS文件外部引用这些变量、函数时
    必须使用exports对象进行暴露。使用者要用require()命令引用这个JS文件。
    
### 模块导出语法
    test这个变量，是一个js文件内部才有作用域的变量。
    如果别人想用这个变量，那么就要用exports进行暴露。
```.js
//foo.js
//定义变量
var test="exportMoudle";
//导出变量
exports.test = test;


var target = require('./foo.js');
console.log(target.test);

//如果暴漏一个构造函数
function Ctr(){
}
module.export = Ctr;
```
