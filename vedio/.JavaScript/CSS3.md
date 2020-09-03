# CSS3



### 1.属性选择器

|div[class="demo"]|div[class^="demo"]|div[class$="demo"]|div[class*="demo"]|
|-|-|-|-|
|选择属性为class并且值等于demo的div元素|选择属性为class并且值以demo开头的div元素|选择属性为class并且值以demo结尾的div元素|选择属性为class并且值包含demo的div元素|

### 2.伪类选择器

1.子元素必须是li
|li:first-child|li:last-child|li:nth-child(2n+1)|li:nth-last-child|li:only-child|
|-|-|-|-|-|
|1.必须是li 2.找第一个 |1.必须是li 2.找最后一个|1.必须是li 2.从第一个开始找n从0开始|1.必须是li 2.从第最后一个开始找n从0开始|1.必须是li 2.只存在唯一一个不存在其他元素|


2.将子元素中li的找出来
|li:first-of-type|li:last-of-type|li:nth-of-type(n+1)|li:nth-last-of-type(n+3)|li:only-of-type|
|-|-|-|-|-|
|1.必须是li 2.找到一类中的第一个 |1.必须是li 2.找一类中最后一个|1.必须是li 2.从第一个开始找一类n从0开始|1.必须是li 2.从第最后一个开始找找一类n从0开始|1.必须是li,2.可以有其他元素单是li必须只有一个|

```.html
<ul>
       <li>1</li>
       <li>2</li>
       <li>3</li>
   </ul>
   <div>55</div>
   <div>66</div>
```
```.css
ul>div{
    background-color: hotpink;
}
ul+div{
 background-color: hotpink;
}
ul~div{
 background-color: hotpink;
}
```

|ul>div|ul+div|ul~div|
|-|-|-|
|直接子元素|兄弟节点|所有后面的元素|


### 3.columns 多列布局
| column-count: 3;|  column-width: 200px;| column-gap: 2px;| column-rule: 3px double ;|
|---|---|---|---|
|列数|每列宽|间隙宽度|分隔线不占宽度|

### 4.盒模型

###### 1.标准盒模型
      
      盒子=content+padding+border

###### 2.IE6混杂盒模型
      
      盒子=content-padding-border


