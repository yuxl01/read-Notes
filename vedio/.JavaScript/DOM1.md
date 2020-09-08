# DOM(一)

### 一、DOM选择器

###### 1.常用选择器

- 1.document.getElementById() 通过选择Id选择器，来选择标签

- 2.document.getElementsByTagName() 通过标签名来选择标签

- 3.document.getElementsByClassName() 通过Class选择器来选择标签

- 4.document.querySelector() 使用css选择语法,选择标签（通过副本的方式选择,不会实时改变）

- 5.document.querySelectorAll() 使用css选择语法,选择返回一组标签（通过副本的方式选择,不会实时改变）

###### 2.Dom属性(节点遍历树)

```.js
<script>
    var dom = document.getElementsByTagName('div')[0];
    var el = dom[0];
    //获取节点类型
    el.nodeType
</script>
```

`节点类型`
|值|节点|描述|
|--|--|--|
|1|元素节点|例如 p标签 和div标签|
|2|属性节点|例如 class id name|
|3|文本节点|例如div标签中的文字或者换行符|
|8|注释节点|注释|

<br>

`属性`

|dom.childNodes|dom.parentNode|dom.firstChild|dom.lastChild|dom.previousSibling|dom.nextSibling|
|-|-|-|-|-|-|
|查找子节点们返回类数组|获取父节点|第一个子节点|最后一个子节点|上一个兄弟节点|下一个兄弟节点|

|dom.firstElementChild|dom.lastElementChild|dom.previousElementSibling|dom.nextElementSibling|
|-|-|-|-|
|第一个子元素节点|最后一个子元素节点|上一个兄弟元素节点|下一个兄弟元素节点|

### DOM树

![原型图片](https://github.com/yuxl01/read-Notes/blob/master/imag/dom.jpg)
