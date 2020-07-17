# CSS 列表模型
> 本文主要对`master`伪元素、`list-item`相关的几个样式属性进行介绍

## `marker` 是什么

`marker`是一个标记伪元素，作用在`list-item`上代表列表项的标志，先附上一个例子，就能很清楚地看出它的作用。

```javascript
<style>
li::marker { content: "(" counter(list-item, lower-roman) ")"; }
li { display: list-item; }
</style>

<ol>
  <li>This is the first item.
  <li>This is the second item.
  <li>This is the third item.
</ol>

// 将会展示
  (i) This is the first item.
 (ii) This is the second item.
(iii) This is the third item.

```
在这里，`marker`为元素定义的是每一项列表项前面的标记符。

##  `marker` 的使用

需要注意的是，普通元素要想使用`marker`，必须将元素定义成`display: list-item`，`list-items`在创建的时候会自动生成`marker`和`counter`。

## 

