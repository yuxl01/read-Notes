# 典型Bug分析

#### 1.Bug #29673
 `N: 1`
 `所属模块：前端`
 `代码段：`
```.ts
    this.messagelist.splice(this.messagelist.indexOf(message),1);    
```

`问题分析：`

    在此处使用indexOf时应该判断根据指定字符串,是否会存在找不到索引的情况，如果找不到那就会返回-1，然后导致splice删除最后一条数据。

`代码根因解析`

    splice：从指定索引开始删除1个长度数据,起始索引为0，为负数就从尾部删除，  每次执行splice将会重新计算索引。
    indexOf：根据指定字符返回字符所占索引号，找不到为-1；

#### 2.Bug #29751

 `N:2 `
 `所属模块：前端`
 `代码段：`

 ```.html
 <div style="width:100%;height:500px;background-color: red;">
    <div style="width:20%;height:200px;background-color: green;float: left;border:1px solid #366da5">
           
    </div>
    <div style="width:80%;height:200px;background-color: yellow;float: left;">
           
    </div>
</div>
 ```

`问题分析：`

    由于外部宽度占100%,内部左浮动div占20%，但是又加了border 1px,左div 总体宽度占（20%+2px）导致会将右边div挤下去。

`代码解析`
    标准盒模型 = 内容区域+border+padding
   
