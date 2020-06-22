# css基础

### 一、主流浏览器及其内核

    浏览器组成为内核+Shell
    浏览器查找节点树自右向左
    
|IE|Firefox|Google chrome|Safari|Opera|
|-|-|-|-|-|
|trident|Gecko|Webkit/blink|webkit|presto|
  
### 二、css基础知识
   
###### 1.css类型
|css文件引入|行间样式|style包裹编写|
|-|-|-|

###### 2.css选择器
|id选择器|class选择器|标签选择器|通配符选择器|属性选择器|父子选择器|直接子元素选择器|并列选择器|分组选择器|
|-|-|-|-|-|-|-|-|-|
|#id|.class|div|'*'|[attrbute="only"]|div span|div>span|div.class必须连着|div,span,em|
|一对一|多对多|标签|所有|包含属性|只注重父子关系成立|选择父级直接子级|两个选择器指向同一个|逗号隔开|


```.css
#only{
   background-color:red;//id选择器
}
.class{
   background-color:red;//类选择器
}
div{
   background-color:red;//标签选择器
}
*{
   background-color:red;//通配符选择器
}
{
 background-color:red;!importtant//加入属性之后优先级最高
}
div span{
 background-color:red;//父子选择器
}
div>span{
 background-color:red;//直接子元素选择器
}
```
 ###### 3.选择器权重
    
  `!importtant > 行间样式 > id选择器 > class选择器|属性选择器 > 标签选择器 > 通配符`
 |importtant|行间样式|id选择器|class 属性 伪类|标签 伪元素|通配符|
 |-|-|-|-|-|-|
 |Infinty正无穷|1000|100|10|1|0|
 
 `小栗子展示红色`
 ```.html
  <div class="classDiv" id="idDiv">
      <p class="classP" id="idP">1</p>
  </div>
 ```
 ```.cs
//10 +10 =20
 .classDiv .classP{
    background-color: green;
}
 //权重100 +1 =101
#idDiv p{
    background-color: red;
}

 ```
 

 
 
