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

  先捕获后冒泡

