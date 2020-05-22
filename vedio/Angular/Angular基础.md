# Angular基础

#### 一、核心特性(组件)

        在Angualr中一切都是围绕组件展开,其实组件就是一个类。
      
###### 1.组件化的意义
        
- 1.分治,有了组件之后可以把逻辑封装在组件内部，避免混在一起。
- 2.复用,封装成组件之后不仅在项目内复用,还可以沉淀作为跨项目复用。


###### 2.组件demo

- 1.@component 组件的装饰器
- 2.selector 用来定义组件渲染的标签的名称（组件名）
- 3.styleUrls 一个数组，用来指定组件样式文件

```.ts
import { Component } from '@angular/core';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})

//组件类 定义逻辑及封装的内容
export class AppComponent {
  title = 'myDemoApp';
 count = 0;
  increment = function(){
       this.count++;
  }

}

```

###### 3.App.Module
  
- 1.declarations组件模块资源：组件,指令,服务.
- 2.imports 依赖模块
- 3.bootstrap 指定启动根组件
 ```.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { FooComponent } from './foo/foo.component';

@NgModule({
  declarations: [
    AppComponent,
    FooComponent,
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

 ```
   
###### 4.templates(模板) 

- 1.数据绑定{{}}
- 2.双向数据绑定[(ngModel)]
- 3.条件渲染
- 4.列表渲染
- 5.指令 


###### 5.元数据
    用来描述组件
    
###### 6.数据绑定
    和vus.js一样，MVVM思想（数据驱动视图）利用特殊的{{}}语法将数据绑定到dom,数据改变会影响视图更新
    
###### 7.指令

 - 1.*NgIf
 - 2. NgFor
 - 3.[(NgModule)]
 
 
###### 8.服务及依赖注入
  ioc
  
