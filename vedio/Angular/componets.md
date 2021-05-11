# 指令

  
   1.组件是什么?  应用程序有序运行依赖不同组件的协作。
   2.指令是什么?  扩展现有元素样式和结构
    
### 一、指令分类
  
  |属性型指令|结构型指令|组件|
  |-|-|-|
  |用于改变目标元素的样式和行为|用于改变目标元素的DOM结构|特殊的指令|
  
  
### 二、指令使用

```.ts
ng g d appHighlight


import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: 'InputItem[type=money]:[money-Text],[Text]',
   host: {
    '(onFocus)': 'onFocus($event.target)',
  },
})
export class AppHighlightDirective {

  constructor(el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'yellow';
 }

onFocus = () => {
   //dosomting
  };
}

```
### 模板<ng-template> 和ngTemplateOutlet
    
    1.ngTemplateOutlet可以在模板中创建内嵌式图
    
`静态嵌入`
```.ts
<div [ngTemplateOutlet]="test"></div>
<ng-template #test>模板 </ng-template>
```
`动态嵌入`
```.ts
  //获取视图引用,static：true为变更检测运行前解析
  @ViewChild('vc', { static: true, read: ViewContainerRef })
  vc!: ViewContainerRef;
  
  //获取模板引用
  @ViewChild('test', { static: true }) tpl!: TemplateRef<void>;


  ngOnInit(): void {
    //将模板动态插入到视图引用实例上
    this.vc.createEmbeddedView(this.tpl);
  }
```
