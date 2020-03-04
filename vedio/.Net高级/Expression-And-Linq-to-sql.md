# Expression And Linq to sql
# 表达式目录树

### 一、什么是表达式目录树

    1、表达式目录树是Linq下的一个Expressions类型
    2、实例化Compile()可以生成一个委托
    3、var不能应用接受表达式目录树声明

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

### 三、解析表达式目录树，生成sql

### 四、表达式树的拼装链接
