# Less混合(Mixins)


### 一.Less混合

`有相同的一系列属性的元素,将公共css提取成一个通用代码块封装。`

`元素demo`

``` .html
 <div class="wrapper">
        <div class="demo" ></div>
        <div class="demo1" ></div>
  </div>
```

###### 1.普通混合

`封装部分会被编译为css代码，带输出。`
   
```.less
//封装同功能
.style{
    width:50px;
    height:100px;
     background-color: red;
}

.wrapper{
    .demo{
      .style;  
    }
    .demo2{
      .style;
    }
}
```

###### 2.不带输出的混合

`封装部分不会被编译为css代码，不带输出。`

```.less
//封装同功能添加()
.style(){
    width:50px;
    height:100px;
     background-color: red;
}

.wrapper{
    .demo{
      .style();  
    }
}
```


###### 3.带参数的混合

`封装部分不会被编译为css代码，不带输出。`

```.less
//封装同功能添加()
.style(@w,@h,@b){
    width:@w;
    height:@h;
    background-color:@b;
}

.wrapper{
    .demo{
      .style(200px,300px,green);  
    }
}
```

###### 4.命名参数


```.less
//封装同功能添加()

//默认参数
.style(@w：20px,@h:40px,@b:pick){
    width:@w;
    height:@h;
    background-color:@b;
}

.wrapper{
    .demo{
      //命名参数
      .style(200px,300px,@b:black);  
    }
}
```

###### 5.匹配模式

     在外部调用.triangle(L,40px,black)时由于匹配模式生效,会自动调用triangle()

```.less
//混合加@_为匹配模式
.triangle(@_){
    width: 0px;
    height: 0px;
}

//带标识符
.triangle(L,@bw,@bc){
    border-width: @bw;
    border-style:solid;
    border-color: transparent @bc  transparent   transparent;
}

.triangle(R,@bw,@bc){
    border-width: @bw;
    border-style:solid;
    border-color: transparent transparent transparent   @bc;
}
```
###### 6.arguments变量

```.less
.border(@1,@2,@3){
    border:@arguments;
}
.wrapper>.sjx{
    .border(1px,solid red);
}
```

### 7.Less计算

```.less
.wrapper > .sjx{
    width: (100px+100);
    height: 200px;
    background-color: red;
}
```