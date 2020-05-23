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


//元组 Tuple
let x: [string, number];
x=['12',3]


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

#### 二、基本语法函数及解构
```.ts
/**
 *提供一个string类型参数和number类型参数
 *返回number
 */
function add(p: string, p2: number): number {
    return p2;
}

//空返回值
function fn():void{
   //todo
}


/**
 *数组解构赋值
 *按照顺序解构
 */
let jg:number[]=[12,3,4,5,6];
let [a,b,c,d] = jg

/**
 *对象解构
 *按照属性名解构 
 */
let user1 ={name:'admin',age:2}
let {name:nikename,age} = user1;

/**
 *函数解构传值*  
 */
function add([x,y]):number{
    return x+y;
}
add([10,20])

/**
 *剩余参数*  
 */
function sub(...arges:number[]){
}
sub(1,2,34);

/**
 *数组展开操作
 *合并返回新数组 
 *输出：[ 1, 2, 3, 4, 5, 6 ]
 */
let arr = [1,2,3];
let arr1 = [4,5,6];
let arrNew = [...arr,...arr1];
console.log(arrNew);
//同样合并数组
let arrNew1 = arr.concat(arr1);

/**
 *对象展开操作
 *合并返回新对象 
 *输出：{ foo: '21', name: 'admin' }
 */
let obj ={foo:"21"}
let obj1 = {
    ...obj,
    name:'admin'
}
console.log(obj1);
```

#### 三、类

    构造函数的另一种书写方式,在ecmascript6中新增。
    
- 1.public 公开访问。
- 2.private 私有访问。
- 3.protected 内部和子类才能使用。
- 4.readonly 只读
- 5.const 常量
- 6.static 只能类本身访问 

|public|private|protected|readonly|const||
    
```.ts
//原来构造对象的方式利用原型
function Persion(name:string,age:number){
    this.name = name
    this.age = age
}
Persion.prototype.SayHello = function():void{
    console.log(this.name,this.age);
}
let p = new Persion("admin",18);
p.SayHello();

//typescript中定义类
class Person {
   private name: string;
   private age: number;
    //ts要求类的成员必须先被定义
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    sayHello(): void {
        console.log(this.name, this.age);
    }
}

//继承父类
class student extends Person{
    constructor(name:string,age:number){
       super(name,age) 
    }
}
let stu = new student("sa",34);
stu.sayHello();

//简写初始化
class jxClass{
    constructor(public name:string,public age:number){}
}
var c = new jxClass('sa',12).name

//属性赋值器
class Student{
    private _name:number;
    get name(){return this._name; }
    set name(val){
        if(val>10){
            throw "赋值失败"
        }
        this._name =val;
    }
}
var s= new Student();
s.name =12;
```
