# CSS 媒体查询

媒体查询为不同设备之间做样式的适配提供了很好的解决方式，也能够提供一些识别不同设备的方式。

## 常规使用

媒体查询可以有以下三种使用方式：
```html
1、 <link rel="stylesheet" type="text/css" media="screen" href="sans-serif.css">
2、 @import url(example.css) screen and (color);
3、 @media screen {
     * { font-family: sans-serif }
    }
```

## 媒体特性(常用)

+ `width: (max-width、min-width)` 适用于视觉和触觉媒体类型，`不能指定负值`

以下媒体查询例子表示该样式表适用于视口（文件渲染的部分屏幕/纸张）宽度在400至700像素之间的设备：
```html
@media screen and (min-width: 400px) and (max-width: 700px) {
  ...
}
```

+ `height: (max-height、min-height)` 适用于视觉和触觉媒体类型，`不能指定负值`

用法与`width`特性一致。

也许你会看到`device-width`和`device-height`这样的媒体特性被使用，这两者更多的是表示设备的物理的宽高，标准文档中指出，建议使用`width`或者`height`去做媒体没查询，实现更好的效果。

+ `orientation (portrait(纵向)、landscape(横向))` 适用于位图媒体类型，表示设备的横屏、竖屏

若`height`媒体特性的值大于`width`媒体特性的值，则`orientation`媒体特性为`portrait`，否则`orientation`为`landscape`。

```html
<!-- 竖屏设备 -->
@media all and (orientation:portrait) {
  ...
}
<!-- 横屏设备 -->
@media all and (orientation:landscape) {
  ...
}
```

+ `aspect-ratio` 设备宽高比，定义为`width`与`height`的比例。

```html
<!-- 表示16/9设备屏幕 -->
@media screen and (aspect-ratio: 16/9) {
  ...
}
```

## any-hover any-pointer
非常强大的功能，可以间接判断是否是触摸设备。
比如`any-hover`判断设备是否支持鼠标经过行为。

+ `any-hover` 

`none`: 没有什么输入装置可以实现hover悬停，即没有鼠标输入设备

`hover`: 一个或多个输入装置可以触发元素的hover悬停交互，即支持鼠标设备

适用场景：在移动端和PC端通用一个按钮，需要给按钮悬停效果，这是PC的hover就不适合移动端了，就可以借助`any-hover`来为移动端做单独的效果优化体验。
```css
button {
    background-color: #fff;
}
button:active {
    background-color: #f0f0f0;
}
/* 兼容PC端： */
@media (any-hover: hover) { 
  button:hover {
    background-color: #f5f5f5;
  }
}
```
+ `any-pointer`

`none`: 没有可点击的设备

`coarse`: 至少一个设备的点击，但是点击不准确，例如手机移动端。

`fine`: 精准点击，常指带鼠标的PC浏览器。

当我们知道一个设备点击是否精确还是不精确之后，我们就能进行相应的体验升级和改进。比方说一个单选框或者复选框，在PC端鼠标点击还是很好操作的，但是在移动端，就需要做优化处理，针对移动端，让复选区域、点击区域变大，从而达到优化用户体验的效果。

+ `两者在浏览器兼容性`

目前来说，除了IE浏览器不兼容，两者在其他浏览器上都能够使用。

## 写在最后
媒体查询的功能远远不止这些，最经典的场景就是对于特殊设备的兼容：iphoneX的安全区域适配。媒体查询在web上为提升用户体验、交互体验举足轻重，只有充分学习它，不断探索与尝试，才能更完美地适配。

## 参考

https://www.w3.org/TR/2020/WD-mediaqueries-5-20200731/<br>
https://www.w3.org/TR/2020/CR-mediaqueries-4-20200721/