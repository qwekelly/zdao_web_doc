# 在vue项目中实现图片打马赛克
使用image-mosaic插件。
## 安装依赖
```javascript
npm install image-mosaic -D
```

## 项目引用
```html
// .vue 文件
<div id="box">
  <canvas id="canvas"></canvas>
  <button id='drawAll'>全部马上</button>
  <button id='clearAll'>还原</button>
</div>
```
```javascript
import Mosaic from 'image-mosaic'

data() {
  return {
    imgSrc: 'xxxx'
  }
},
created() {
  this.initMosaic()
},
methods： {
  drawImageToCanvas () {
    const canvas = document.querySelector('#canvas')
    const ctx = canvas.getContext('2d')
    let imageUrl
    if (this.imgSrc) {
      imageUrl = this.imgSrc
    }
    return new Promise((resolve, reject) => {
      const image = new Image()
      image.crossOrigin = 'Annoymous'
      image.onload = function () {
        canvas.width = image.width
        canvas.height = image.height
        ctx.drawImage(this, 0, 0, image.width, image.height)
        resolve(ctx)
      }
      image.src = imageUrl
    })
  },
  initMosaic () {
    this.drawImageToCanvas().then(ctx => {
      const mosaic = new Mosaic(ctx)
      const MouseEvents = {
        init () {
          mosaic.context.canvas.addEventListener('mousedown', MouseEvents.mousedown)
        },
        mousedown () {
          mosaic.context.canvas.addEventListener('mousemove', MouseEvents.mousemove)
          document.addEventListener('mouseup', MouseEvents.mouseup)
        },
        mousemove (e) {
          if (e.shiftKey) {
            mosaic.eraseTileByPoint(e.layerX, e.layerY)
            return
          }
          mosaic.drawTileByPoint(e.layerX, e.layerY)
        },
        mouseup () {
          mosaic.context.canvas.removeEventListener('mousemove', MouseEvents.mousemove)
          document.removeEventListener('mouseup', MouseEvents.mouseup)
        }
      }
      MouseEvents.init()
      document.querySelector('#drawAll').addEventListener('click', () => {
        mosaic.drawAllTiles()
      })
      document.querySelector('#clearAll').addEventListener('click', () => {
        mosaic.eraseAllTiles()
      })
    })
  }
}
```
## 实现功能：

画笔式拖动打码、一键全局打码、还原功能

如果需要获取打码后的图片，可以采用以下方式
```javascript
var dataURL = document.querySelector('#canvas').toDataURL('image/png')
```