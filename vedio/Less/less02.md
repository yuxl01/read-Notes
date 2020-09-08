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