# Less基础


### 1.less中的注释

    1.以//开头的注释不会被编译到CSS文件中。
    2.以/**/包裹的注释会被编译到CSS文件中去。
    

### 2.less中的变量

`1.值变量定义为@ctr：red,那么用的时候就直接写@ctr`
<br>

`2.定义变量作为选择器或者属性名在用的时候需要加一个大括号，例如@{select} `

``` .less
@clr:red;
@select:.wrapper;
@m:background-color;

@{select}{
    border: 1px solid black;
    width: 300px;
    height: 500px;
    margin: 0 auto;
    .demo{
        position:absolute;
        left:0;
        right: 0;
        top:0;
        bottom: 0;
        margin:auto;
        @{m}: @clr;
        height: 100px;
        width: 100px;
    }
}
```
    
`3.变量的延迟加载`

```.less
@var:0;
.class{
    @var:1;
    .bars{
        @var:2;
        three:@var;
        @var:3;
    }
    one:@var
}

//less作用域时块级的,又因为延迟加载所以bars中three的值为3
//又因为one的作用域在.class所以为1
```


### 3.嵌套规则

```.less
//如果不添加&将会编译为并列选择器
//必须使用&符号才能输出css为.wrapper:hover
.wrapper{
    border: 1px solid black;
    width: 300px;
    height: 500px;
    margin: 0 auto;
    &:hover{
        background-color: green;
    }
} 
```


    1.基本嵌套规则(父子级),b假设是a的子级选择器 那就直接在a里面定义b{}
    2.&的使用代表平级 ,上面输出 .wrapper:hover {}




### 4.导入（Importing）

    可以导入一个 .less 文件，此文件中的所有变量就可以全部使用,如果导入的文件是 .less 扩展名，则可以将扩展名省略
    
   1. @import "library"; // library.less
   2. @import "typo.css";
