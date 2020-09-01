# CSS3


### 1.伪类选择器

1.子元素必须是li
|li:first-child|li:last-child|li:nth-child(2n+1)|li:nth-last-child|li:only-child|
|-|-|-|-|-|
|1.必须是li 2.找第一个 |1.必须是li 2.找最后一个|1.必须是li 2.从第一个开始找n从0开始|1.必须是li 2.从第最后一个开始找n从0开始|1.必须是li 2.只存在唯一一个不存在其他元素|


2.将子元素中li的找出来
|li:first-of-type|li:last-of-type|li:nth-of-type(n+1)|li:nth-last-of-type(n+3)|li:only-of-type|
|-|-|-|-|-|
|1.必须是li 2.找到一类中的第一个 |1.必须是li 2.找一类中最后一个|1.必须是li 2.从第一个开始找一类n从0开始|1.必须是li 2.从第最后一个开始找找一类n从0开始|1.必须是li,2.可以有其他元素单是li必须只有一个|

