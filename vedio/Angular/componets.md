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
  selector: '[appAppHighlight]'
})
export class AppHighlightDirective {

  constructor(el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'yellow';
 }

}

```
