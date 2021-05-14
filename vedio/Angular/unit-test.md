### 单元测试

 `单元测试的纯粹目的就是为了减少bug......`


#### 1.基本定义

|名词|定义|描述|
|-|-|-|
|Karma|执行过程管理工具|可用于测试所有主流Web浏览器，也可集成到CI工具|
|Jasmine|单元测试框架|用Karma让Jasmine测试自动化完成,Jasmine用来编写Js测试的框架,不依赖于任何其它的js框架,也不需要对DOM操作 |



#### 2.常用概念

|名称|描述|
|-|-|
|describe()|测试入口,含有2个参数,p1类型string，p2类型是一个function,p1代表给你这个测试用例取个名字，p2里边就是你的测试代码|
|it()|用it函数开始测试点spec,含有2个参数,p1类型string，p2类型是一个function,p1代表给你这个测试点取个名字,p2里边就是你的测试代码,一个describe()中可以包含n个it(),并且在describe()定义的变量在it中可以直接使用|
|expectations|断言,可以返回true和false,一个测试点(spec)可以包含多个断言,全部的断言返回true这个测试点就通过|
|Matchers|匹配校验规则,jasmine中有很多Matchers方法给我使用,我们也可以定义自己的Matchers方法|
|beforeEach()|为了代码简洁,减少重复性的工作 beforeEach会在每个spec之前执行|
|afterEach()|为了代码简洁,减少重复性的工作 afterEach会在每个spec之后执行|


```.js
//一个最基本的测试用例
//describe和it作为方法是可以嵌套的,describe中可以出现子describe和it
describe("AppComponent", () => {
  //befor 执行...
  //测试的case 1
  it("case 1", () => {});
  //测试的case 2
  //.....
  //测试的case n+

  it("should ngModel work", () => {
    component.modelValue = 4;
    let a = 4;
    expect(component.modelValue).toBe(a);
    expect(a).not.toBe(null);
  });
});
```

#### 3.常用匹配规则

    1.Matcher方法带着一个期望值，如上面的true。Matchers方法返回true或者false
    2.它决定着测试点（spec）是否通过。所有的Matchers方法都能在mathcer前面加上not来进行否定断言

|规则|描述|
|--|--|
|expect(a).toBe(b);expect(a).not.toBe(null);|比较a、b是否相等；验证a是否不是空|
|expect(foo).toEqual(bar);|比较了两个对象是否相等|
|expect(message).toMatch(/bar/);|正则匹配|
|expect(a.foo).toBeDefined();|验证变量是否被定义  |
|  expect(a).toContain('bar');|验证a是否包含bar属性|
|expect.toBeGreaterThan,toBeLessThan|比较大小|
| expect(bar).toThrow();|测试一个方法是否抛出异常|
|expect().toBeTruthy()|是否为true|
|expect().toBeFalsy()|是否为false|


#### 4.The Runner and Reporter

`所有的spec都可以在页面中运行，这个页面就叫做Runner`

```.js
var htmlReporter = new jasmine.HtmlReporter(); //创建一个HTMLReporter
    jasmineEnv.addReporter(htmlReporter);  

    jasmineEnv.specFilter = function(spec) {  //一个过滤器，允许我们点击单个的suites，单独运行
    return htmlReporter.specFilter(spec);
    };  


    var currentWindowOnload = window.onload;   //页面加载完毕后，执行所有的test。
    window.onload = function() {
        if (currentWindowOnload) {
            currentWindowOnload();
        }

        document.querySelector('.version').innerHTML = jasmineEnv.versionString();
        execJasmine();
    };

    function execJasmine() {
            jasmineEnv.execute();
    }
    })();
```
