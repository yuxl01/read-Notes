# 典型Bug分析

 `N: Bug #29673`
 `所属模块：前端`
 `代码段：`
```.ts
    this.messagelist.splice(this.messagelist.indexOf(message),1);    
```
`代码解析`

    splice：从指定索引开始删除1个长度数据,起始索引为0，为负数就从尾部删除，  每次执行splice将会重新计算索引。
    indexOf：根据指定字符返回字符所占索引号，找不到为-1；

`问题分析：`

    在此处使用indexOf时应该判断根据指定字符串,是否会存在找不到索引的情况，如果找不到那就会返回-1，然后导致splice删除最后一条数据。
