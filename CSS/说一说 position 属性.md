# CSS position

CSS position属性用于指定一个元素在文档中的定位方式。top，right，bottom 和 left 属性则决定了该元素的最终位置。

## 常用语法

position: static | relative | absolute | sticky | fixed    
默认值：static，没有继承性。

`static`: 表示指定元素使用正常的布局行为。此时 top, right, bottom, left 和 z-index 属性无效。

`relative`: 相对定位的元素是在文档中的正常位置偏移给定的值，但是不影响其他元素的偏移。

`absolute`: 元素会被移出正常文档流，绝对定位元素相对于最近的非 static 祖先元素定位。当这样的祖先元素不存在时，则相对于ICB（inital container block, 初始包含块）。

`fixed`: 固定定位与绝对定位类似，只不过固定定位相对于屏幕视口（viewport）的位置来指定元素位置。

`sticky`: 粘性定位可以说是相对定位和固定定位的混合，相对它的最近滚动祖先（nearest scrolling ancestor）和 containing block (最近块级祖先 nearest block-level ancestor)偏移。

## 相对定位
当一个元素设置为相对定位，而且同时设置`left`和`right`属性值时，定位值遵循这么一个规则：
当两者都为auto时，定位置不起效果；当有一方auto时，以另一方为定位置；当两者都不为auto时，优先`left`为定位置。

所以以下三条规则等效，并将方框向左移动1em：
```css
div { position: relative; left: -1em; right: auto }
div { position: relative; left: auto; right: 1em }
div { position: relative; left: -1em; right: 5em }
```

## 绝对定位
绝对定位元素相对于最近的拥有 position 属性但属性值不是 static 的祖先元素定位。当这样的祖先元素不存在时，则相对于ICB（inital container block, 初始包含块，即 viewport 视口）
```css
div { position: absolute; left: 50px; top: 50px; }
```

## 粘性定位
```css
div { position: -webkit-sticky; position: sticky; top: 10px; }
```
在 viewport 视口滚动到元素 top 距离小于 10px 之前，元素为相对定位。之后，元素将固定在与顶部距离 10px 的位置。

须指定 top, right, bottom 或 left 四个值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。


## 固定定位
在下面的示例中，元素始终定位在离页面顶部 10px 的地方。
```css
div { position: fixed; top: 10px; }
```

## 参考

https://www.w3.org/TR/2020/WD-css-position-3-20200519
