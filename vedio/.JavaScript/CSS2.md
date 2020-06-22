# css Ⅱ

### 一.常用css属性

###### 1.em
    
    1em = 1*font-size(自身);


```.css
//单行文本垂直居中
line-height高度等于容器高度

//首行缩进
text-indent:2em;

//文字加横线
text-decoration: line-through;

//去掉横线
text-decoration: none;

//下划线
text-decoration: underline;

//鼠标移入展示图标
cursor: copy;

//伪类选择器hover 鼠标移入
a:hover{
  background-color: orange;
}

//圆角
border-radius: 10px;
``` 

 ### 二、标签 
 
 ###### 1.行级元素 
    <span a storng em >
    内容决定元素所占位置,不可以改变宽高。
    
 ###### 2.块级元素 
    <div p ul li form>
    独占一行,可以改变宽高。
    
 ###### 3.行级块元素 
    <img 图片布局只设置一个>
    内容决定大小,可以改变宽高。
