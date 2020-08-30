# CSS 高效优化策略之编写风格

一般谈论到效率、性能问题，我们都很少想到CSS在请求回来之后，在浏览器进行CSS渲染的效率是怎样的。本文来介绍一下对于CSS的一些高效优化策略,可以提高我们页面的CSS渲染效率.

## 从右到左

值得注意的是,渲染引擎解析 CSS 选择器时是从右往左解析的,这样做是为了减少无效匹配次数，从而匹配快、性能更优。

所以，我们在书写 CSS Selector 时，从右向左的 Selector Term 匹配节点越少越好。因此在编写CSS时就要避免深层次的DOM结构,还有一般不要超过3层的CSS匹配嵌套，比如：

```css
div > div > div > p {color:red;}
/* 或者 */
.class1 {
  .class2 {
    .class3 {
      .class4 {
        color:red;
      }
    }
  }
}
```

## ID选择器是最高效的

因为id是唯一性的,因此在CSS渲染是匹配效率非常高。不过考虑到CSS代码的复用性，使用id selector又有点不好复用，因此一般推荐使用class selector。

而attribute selector的匹配是最慢的，尤其是`p[id="id1"]`这种，明明有id，可以使用效率更高的id选择器，却偏偏要降几个维度。

到这里，结合前面从右到左规则和ID选择器的高效匹配，我们就会发现下面这种写法的效率是不高的。

```css
#id_name > em {   }
```
哪怕id选择器的效率再高，但是渲染的时候始终是先渲染`<em>`标签，这一层就已经卡住了时间，效率自然就不会高。

## 避免使用后代选择

```css
html body ul li a {  }
```

## computed Style

浏览器通过共享computed Style，能够极大地提升执行效率。如何高效共享 Computed Style ？说白了就是建立公共类样式，让这些样式能够复用。不过也要注意以下几点：
+ TagName 和 Class 属性必须一样
+ 不能有 Style 属性。哪怕 Style 属性相等，他们也不共享。
+ 不能使用 Sibling selector，譬如: first-child、 :last-selector、 ChildSelector。
+ mappedAttribute 必须相等。

## 良好的书写顺序避免过分回流/重排

浏览器并不是一获取到 CSS 样式就立马开始解析，而是根据 CSS 样式的书写顺序将之按照 DOM 树的结构分布渲染样式，然后开始遍历每个树结点的 CSS 样式进行解析，此时的 CSS 样式的遍历顺序完全是按照之前的书写顺序。


`因此，在解析过程中，一旦浏览器发现某个元素的定位变化影响布局，则需要倒回去重新渲染。`

比方说：
```css
.class_name {
  width: 150px;
  height: 150px;
  font-size: 24px;
  position: absolute;
}
```
当浏览器解析到 position 的时候突然发现该元素是绝对定位元素需要脱离文档流，而之前却是按照普通元素进行解析的，所以不得不重新渲染。

我们对代码进行调整：这样就能让渲染引擎更高效的工作
```css
.class_name {
  position: absolute;
  width: 150px;
  height: 150px;
  font-size: 24px;
}
```

对于编写的顺序，可以参考以下：
+ 定位属性
```
position  display  float  left  top  right  bottom   overflow  clear   z-index
```

+ 自身属性
```
width  height  padding  border  margin   background
```
+ 文字样式
```
font-family   font-size   font-style   font-weight   font-varient   color
```
+ 文本属性
```
text-align   vertical-align   text-wrap   text-transform   text-indent    text-decoration   letter-spacing    word-spacing    white-space   text-overflow
```

+ CSS3 中新增属性
```
content   box-shadow   border-radius  transform
```

## 依赖继承

某些属性可以继承，就没有必要再写一遍，一般能够继承的属性有以下：
+ font-family、font-size、font-weight 等 f 开头的 CSS 样式。
+ text-align、text-indent 等 t 开头的样式。
+ color。

不过要注意的是，继承只发生在原生标签之间，如果使用了第三方框架，当你希望框架的组件也继承父级的属性，这是不现实的，因为框架组件内部通常会定义一些自己通用的全局样式。

## 其他规则

还有其他一些编写规则，比方说：
1、将浏览器前缀置于前面，将标准样式属性置于最后。
2、遵守 CSSLint 规则。
3、减少 CSS 文档体积。
4、避免使用 @import。等...

不过，如果你的项目使用了Sacc或者Scss之类的CSS预处理器，那么这些问题你都可以忽略,因为这些预处理器已经帮我们把CSS优化到最佳状态再进行打包，供浏览器渲染。

## 参考

https://css-tricks.com/efficiently-rendering-css