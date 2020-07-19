# CSS 列表模型之counter计数器

> 计数器是一种特殊的数字跟踪器，通常用于为CSS列表项自动编号。你可以在项目中通过`counter-increment`、`counter-set`、`counter-reset`来创建一个计数器，并在`counter()/counters()`中使用。

在CSS语法中，需要定义一个`counter-name`作为计数器的唯一标识，这是必不可少的。解析计数器一般有以下几个步骤：

+ 当前计数器从父元素继承而来，遵循父元素的计数规则
+ 通过`counter-reset`实例化一个新的计数器
+ 通过`counter-increment`设置计数器的递增值
+ 通过`counter-set`为计数器设置计数初始值
+ 通过`counter()/counters()`使用计数器
