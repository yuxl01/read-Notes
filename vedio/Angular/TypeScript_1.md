# TypeScript = Type+ EcmaScript
  
  - 1.类似js语言,增强js部分语法。
  - 2.遵循ecmascript规范，由微软开发
  - 3.ts可以编译成js从而支持js环境运行
  - 4.和js关系如同less和css
  
   
#### 一、基本语法赋值

```.ts
//赋值变量
var a:string ='str'
  
//对象赋值定义类型约束
var user: {
    name: string,
    age: number
} = {

    name: 'jack',
    age: 12
}
//模板字符串
let tmp1:string = `测试${user.name}`

//定义数组
let arr:Array<string> = ["1","2","3"]
let arr1:number[] =[1,2,34,5]


//接口约束类型
interface Person {
    name: string;
    age: number
}
let p: Person = {
    name: 'asa',
    age: 34,
}

//在不确定的时候可以用,尽量少用，也不会有智能提示
let c:any=123;
c='2'
//转换
let ret:string = (c as string).substr(1);
```

#### 二、基本语法函数
```.ts
//提供一个string类型参数和number类型参数
//返回number
function add(p: string, p2: number): number {
    return p2;
}
//空返回值
function fn():void{
   //todo
}
```
