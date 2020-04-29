# MVC 

### 一、基础知识

###### 1.Application_Start

    1. 网站启动时，响应第一次请求的执行的
    2. 合适做一些初始化
    
###### 2.RouteConfig路由匹配从上往下
        
    1.routes.MapRoute：注册路由
    2.routes.IgnoreRoute：忽略路由
    
    

### 一、 数据传值的多种方式

###### 1.ViewData 
    
    是定义在ControllerBase中的一个ViewDataDictionary类型的字典,在传值的时候键对应的值可以是对象也可以是值。
    
```.cs
 base.ViewData["User1"] = new User()
 {
     Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
 }; 
 
 base.ViewData["doSomting"] = "sw";
```
    
###### 2.ViewBag

    是定义在ControllerBase中的一个dynamic类型的属性,在传值的时候可以是任意类型,并且编译器将不检查。
    
 ```.cs
  base.ViewBag.Name = "admin";
  base.ViewBag.User = new User()
  {
      Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
  };         
```   
###### 3.TempData 

     是定义在ControllerBase中的一个TempDataDictionary类型的字典,基于session可以跨控制器action(存在一次)
     
 ```.cs
 base.TempData["User"] = new User()
 {
     Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
 };
``` 
