# typeOf 类型转换

      typeof(param):判断输入param的类型,结果值情况：返回字符串描述的数据类型

### 一、显示类型转换

###### `1、Number(param)`
   
      Number(param)转换数字,可以转换bool,字符串数字,null(转换为0),undefine转换为NaN.
###### `2、parseInt(param,radix)`
      1、parseInt(param,radix)转换字符串数字,浮点型为整型，其他的为NaN.
      2、从数字位开始看识别到非数字结束,100px取100可以用parseInt('100px').
      3、第二个参数以目标数字为基底转化为10进制.
```.js
   //声明一个2进制字符串
   var bitStr ='10';
   //利用parseInt将目标进制转换为10进制
   var result = parseInt(bitStr,2);
   console.log(result);
   //输出结果 2
```
###### `3、String(param)和toString(radix)`
      1、String(param)转换任何类型为字符串，包括null和undefine.
      2、toString()也可以转换为字符串，类似c#中的扩展方法 str.toString(),但是null和undefine使用会报错。
      3、toString()带参数的就会将字符串以10进制转换为目标进制     
 ```.js
   //声明一个数字类型12
   var dStr =12;
   //利用toString(16)将目标进制转换为16进制
   var result = dStr.toString(16);
   console.log(result);
   //输出结果 c
```  

### 二、隐式类型转换      

###### `1、isNaN()/+ - (正负)`
      1、内部隐式调用Number(),返回bool
      
 ```.js
  //返回false
  isNaN('123') 
  //返回true
  isNaN('a') 
  //返回false
  isNaN(null) 
  //返回true
  isNaN(undefined)
  
  //a变为number类型
  var a =+'b';
  typeof(a)
  
```  

###### `2、特殊的转换`
      
      1、==会发生类型转换.
      2、===绝对比较不会发生转换.
      3、NaN不管是否有类型转换都为false.
      4、变量未声明,只有当typeof()使用时才不会报错。
      5、typeof(undefined)返回'undefined',typeof(typeof(undefined))返回string,typeof(NaN)为'number'。
      
 ```.js
  //返回false
  undefined>0
  undefined<0
  undefined==0
  
  //返回false
  null>0
  null<0
  null==0
  
  //返回true
  null==undefined
  //返回false
  NaN==undefined
```  
     
