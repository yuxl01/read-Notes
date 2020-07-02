# Vue

### 1、基本介绍

### 2、简单实现

###### 1.响应的数据绑定
  Object.defineProperty
  
###### 2.虚拟Dom的js实现

```.js
//节点绑定实现
function vElement(tagName, prop, children) {
    if (!(this instanceof vElement)) {
        return new vElement(tagName, prop, children);
    }
    if (Object.prototype.toString.call(prop) === "[object Array]") {
        children = prop;
        prop = {};
    }
    this.tagName = tagName;
    this.children = children;
    this.prop = prop;
    var count = 0;
    this.children.forEach(function (child, index) {
        if (child instanceof vElement) 
            count += child.count;
        count++;
    })
    this.count = count;
}
//生成节点
vElement.prototype.render=function(){
    var el = document.createElement(this.tagName);
    var children = this.children;
    var prop = this.prop;

    for(var item in prop)
    {
        var curProp = prop[item];
        el.setAttribute(item,curProp);
    }
    children.forEach(function(child,index){
        if(child instanceof vElement)
        {
            var childDom = child.render();

        }else{
            var childDom = document.createTextNode(child);
        }
        el.appendChild(childDom);
    })
    return el;
}
var dom = vElement("div", { class: "demo" }, ["hello world", vElement("p", { class: "demo2" }, ["我是P标签"])]);
console.log(dom.render());
```
