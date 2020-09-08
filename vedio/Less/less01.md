# Less基础


### less中的注释

    1.以//开头的注释不会被编译到CSS文件中。
    2.以/**/包裹的注释会被编译到CSS文件中去。

### less中的变量

    1.值变量 @var
    2.属性选择器变量 @select 使用时需要加入{} 

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