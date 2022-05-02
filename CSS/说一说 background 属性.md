# CSS background 属性


## background-color与 background设置背景颜色的区别

background-color设置的只是背景色，background设置的是整个背景。

当元素本身设置了background-image属性时，如果设置了background-color，图片不会被覆盖，background-color会在image底层；如果设置的是background，那么图片会被颜色给覆盖掉。

如下图所示，是分别设置了background-image和background-color的效果。如果设置的background: black，那么图片将会被black覆盖掉，但是图片仍然存在，只是被覆盖了而已。

![background_image_color](./images/background_image_color.png "background_image_color")


## background-attachment

定义background-image是否跟随容器的滚动而滚动。对background-color无效。

background-attachment = scroll | fixed | local

当background-attachment: fixed时，设置的background-image背景不随元素滚动而滚动。

## background-position

定义background-image在容器内的位置。对background-color无效。

background-position = [ left | center | right | top | bottom | length-percentage ]

```css
body { background-positon: right top }    /* 100%   0% */
body { background-positon: top center }   /*  50%   0% */
body { background-positon: center }       /*  50%  50% */
body { background-positon: bottom }       /*  50% 100% */
```
下图所示是background-positon: center的效果：
![background_position.png](./images/background_position.png "background_position.png")


## visibility: hidden与display: none对background-image的影响

background-image对应的是一张静态资源图片，当页面渲染前会请求这张图片资源回来进行渲染。

visibility: hidden，元素仍然存在DOM树中，只是不显示而已；display: none，元素不存在与DOM树中。

当元素visibility: hidden时，这张图片资源仍然会请求，但是不会显示出来；当元素设置display: none时，这张图片资源不会请求，也不会显示。还有一种场景：如果页面出现了滚动条，那么滚动条下面(即视图之外)的background-image静态资源会不会请求呢？答案是会的。只要是元素存在于DOM树上，不论显示还是隐藏，其依赖的banckground-image资源都将会请求回来。

从这里可以引发一些对资源请求性能的思考了。

## 参考：

https://www.w3.org/TR/css-backgrounds-3/