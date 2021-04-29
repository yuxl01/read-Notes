### 一、事件基础

`var div =  document.getElementsByTagName('div')[0];`

```.js

div.addEventListener('click',function(){
  alert("IE9一下不兼容");   
},false);


div.attachEvent('onClick',function(){
  alert("IE9兼容");   
},false);

```



### 二、事件模型
    
    `捕获和冒泡同时绑定那一定是先捕获后冒泡,有些事件不能冒泡 例如onblur()`
    
###### 1.事件冒泡

  `结构上并非视觉存在父子关系的元素会存在事件冒泡,自内向外`
  
```.js
        var s = document.getElementsByClassName('s')[0];
        var l = document.getElementsByClassName('l')[0];
        var m = document.getElementsByClassName('m')[0];

        s.addEventListener('click', function () {
            console.log("红色");
        }, false)
        l.addEventListener('click', function () {
            console.log("绿色");
        }, false)
        m.addEventListener('click', function (event) {
            console.log("蓝色");
        }, false)
```
###### 2.事件捕获

  `自外向内传递`

```.js
        var s = document.getElementsByClassName('s')[0];
        var l = document.getElementsByClassName('l')[0];
        var m = document.getElementsByClassName('m')[0];

        s.addEventListener('click', function () {
            console.log("红色");
        }, true)
        l.addEventListener('click', function () {
            console.log("绿色");
        }, true)
        m.addEventListener('click', function (event) {
            console.log("蓝色");
        }, true)
```
###### 3.取消事件模型

  1.取消事件冒泡：e.cancelBubble =true Ie独有谷歌已经实现   e.stopPropagation();   
