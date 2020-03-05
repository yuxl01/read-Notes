# Expression And Linq to sql
# 表达式目录树

### 一、什么是表达式目录树

    1、表达式目录树是Linq下的一个Expressions类型
    2、实例化Compile()可以生成一个委托
    3、var不能应用接受表达式目录树声明
    
    通俗的理解表达式目录树就是一种结构,可以自定义拼接，然后编译实现。

##### 1、表达式目录树类型
    
    1、Expression<Func<int, int, int>> exp = (m, n) => m * n - 5
    
```.cs

//声明参数a
ParameterExpression f = Expression.Parameter(typeof(int),"a");
//声明参数b
ParameterExpression s = Expression.Parameter(typeof(int),"b");
//构建左树
BinaryExpression left = Expression.Multiply(f,s);
//声明一个常量
ConstantExpression c = Expression.Constant(5, typeof(int));
//构建右树
BinaryExpression right = Expression.Subtract(left, c);
//拼接表达式
Expression<Func<int,int,int>> expression=Expression.Lambda<Func<int,int,int>>(right,new ParameterExpression[]
{
    f,s
});
//Compile编译表达式调用
int result =   expression.Compile()(2,3);  

```

### 二、动态拼装表达目录树和扩展应用

    1、动态拼装表达式常用基本用法
```.cs
//声明一个参数表达式
ParameterExpression param = Expression.Parameter(typeof(string),"o");
//声明一个常量表达式
ConstantExpression  const   ConstantExpression cont = Expression.Constant("5",typeof(string));
//声明表示访问类属性或者类字段 (拼接o.Id)
MemberExpression filed = Expression.Field(param,typeof(object).GetField("Id"));

//声明一个被表达式调用的无参ToString方法
MethodInfo mi = typeof(Int32).GetMethod("ToString", new Type[] { });
//调用无参数函数的表达式 (拼接o.Id.ToString())
MethodCallExpression m = Expression.Call(p, mi);

//声明一个被表达式调用的方法变量默认参数为string类型
MethodInfo mi2 =typeof(Int32).GetMethod("Equals",new Type[] {typeof(string) });
//声明一个传入调用方法参数的表达式
Expression[] expressionarr = new Expression[] { const };
//调用有参函数表达式
MethodCallExpression m = Expression.Call(m,mi2,expressionarr);

//拼接Lambda传入参数表达式 为编译做准备
Expression[] expressionarr1 = new Expression[] { param };
Expression<Func<People, bool>> expression = Expression.Lambda<Func<People, bool>>(m,expressionarr1);
//编译表达式
expression.Compile()
```



### 三、解析表达式目录树，生成sql

### 四、表达式树的拼装链接
