# 聊一聊CSS overflow


## overflow
定义内容溢出框的处理方法，主要有以下几个值：

属性值|描述
--|:--:
visible | 默认值，元素的内容在元素框之外也可见
hidden | 元素的内容会在边界处被裁减，超出裁剪区域的内容不可见
clip | 完全禁止通过任何机制滚动，此时元素框不属于滚动容器
scroll | 元素的内容会在边界处被裁减，并显示滚动条来适应内容
auto | 如果内容被裁减，则会显示滚动条来适应内容

## overflow-x 和 overflow-y
可以使用overflow-x和overflow-y属性水平或垂直地定义内容溢出。例如在下面的代码，可以滚动水平溢出，同时隐藏超出框高的文本：
```css
.box {
  overflow-y: hidden;
  overflow-x: scroll;
}
```

## 清除浮动
你可以使用overflow来清除浮动：设置除了visible的属性值，都可以实现清除浮动的效果。
```html
<div class="bg">
	<div class="float_left"></div>
</div>
<style>
    .bg{
		overflow: hidden;
	}
	.float_left{
		float: left;
	}
</style>
```

## text-overflow
设置文本溢出的处理方式，处理当文本溢出元素的框时文本被剪裁的情况。

使用的时候有以下几点需要注意：
+ text-overflow 只对block元素起作用
+ 只有当容器的overflow属性的值为hidden、scroll或auto和white space:nowrap；时，才会发生文本溢出

单行文本溢出：
```html
<div class="bg">找商机，找人脉，用找到APP</div>
<style>
.bg{
    overflow: hidden;
    margin: 50px auto;
    width: 200px;
    white-space: nowrap;
    text-overflow: ellipsis;
}
</style>
```
多行文本溢出：（下面实例设置2行）
`    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
`

```html
<div class="bg">找商机，找人脉，用找到APP</div>
<style>
.bg{
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    margin: 50px auto;
    width: 200px;
    text-overflow: ellipsis;
}
</style>
```

## overscroll-behavior
设置DOM元素滚动到边界时候的行为。通常用于阻止DOM元素滚动影响页面滚动的情况发生。

`auto`：默认值，滚动到边缘后继续滚动外部的可滚动容器。

`contain`：滚动行为只会发生在当前元素的内部，不会对相邻滚动区域或者祖先滚动区域进行滚动。

`none`：相邻的滚动区域不会发生滚动，并且会阻止默认的滚动溢出行为。

## scroll-behavior
`scroll-behavior:smooth` 写在滚动容器元素上，可以让容器（非鼠标手势触发）的滚动变得平滑。替代JS的scrollIntoView()来实现平滑滚动效果。

`auto`：默认值，正常滚动。

`smooth`：平滑滚动。

更简单更实际的用途：想要实现滚动效果平滑，在滚动的元素上加上`scroll-behavior:smooth` ，比较适用于同一个overflow容器下的锚点滚动，不适用于分页下请求新数据回来导致overflow容器重新渲染的场景。
```css
.box {
    scroll-behavior: smooth; 
    overflow: hidden; 
}
```


## 参考

https://www.w3.org/TR/2020/WD-css-overflow-3-20200603/

https://css-tricks.com/almanac/properties/o/overscroll-behavior/

https://css-tricks.com/almanac/properties/s/scroll-behavior/
