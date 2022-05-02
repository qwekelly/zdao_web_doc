#### 1.对比`<a>`与`<router-link>`标签的区别

`<router-link>`组件避免了不必要的重渲染，只更新变化的部分从而减少DOM性能的消耗，利用虚拟DOM和diff算法实现了对页面的按需更新。`<a>`标签则体现为页面的消失-重现，会进行大量的DOM元素重新渲染，倘若在一个比较大的项目里，将会消耗大量的DOM性能。<br>
除此，`<router-link>`的点击事件`@click`和`@mouseover`默认是会被阻止的。如果想要添加点击事件，Vue 提供了一种解决方案，在事件上加上`.native`修饰符 `，如`@click.native`即可。
 

 #### 2.基于rollup对vue进行封装自己的npm组件包

 <a href="https://github.com/qwekelly/vue-components">参考项目</a><br>
 项目引入vue包，但是不将vue进行打包，需要在rollup.config.js文件里面进行配置（具体参考项目）。封装vue组件，需要用到rollup-plugin-vue插件。<br>
 1.在打包的时候会出现一个错误：在dist目标文件代码中会出现require路径为本地路径的情况。这个时候就需要配置一下<a href="https://rollup-plugin-vue.vuejs.org/options.html">rollup-plugin-vue </a>的属性含义。
 ```
 vue({
      needMap : false,
      normalizer : '~rollup-plugin-vue/runtime/normalize',
      styleInjector : '~rollup-plugin-vue/runtime/browser',
    }),
 ```
 推荐官方文档<a href="https://rollup-plugin-vue.vuejs.org/">rollup-plugin-vue.vuejs.org</a>提供了一种封装方式。<br>
 2.在对打包文件进行压缩时，rullop提供两种方式：
 <a href="https://github.com/TrySound/rollup-plugin-uglify">rollup-plugin-uglify </a>和
 <a href="https://github.com/TrySound/rollup-plugin-terser">rollup-plugin-terser </a>。区别在于uglify只能对ES5语法进行识别压缩，不支持ES6+的压缩；对于使用到了ES6语法的项目，推荐使用terser。<br>
 3. <a href="https://github.com/TrySound/rollup-plugin-eslint">rollup-plugin-eslint </a> 对项目进行eslint检测，不过涉及到.vue文件的eslint识别，目前该插件还不支持.vue进行识别。在github上的issues上有提到。



### 构造器模式
> 在面向对象编程中，构造器是一个当新建对象的内存被分配后，用来初始化该对象的一个特殊函数。<br>

简单地说：`var newObject = new Object()` 中的 `Object` 就是一个构造器。javascript 中通常用 `new` 把函数当作构造器使用，回到对象创建，一个基本的构造函数看起来像这样：
```
function Student(name, code, age) {
  this.name = name;
  this.code = code;
  this.age = sge;
  this.toString = function() {
    return this.name + 'is' + this.age + 'years old';
  }
}

var studentA = new Student('xiaomi', 98, 18);
console.log(studentA.toString());
```
上面是简单版本的构造器模式，存在难以继承的问题，每次创建一个对象时，toString()之类的函数都被重新定义，这不是非常好。可以采用函数的prototype属性来拓展优化，如下：
```
function Student(name, code, age) {
  this.name = name;
  this.code = code;
  this.age = sge;
}

Student.prototype.toString = function() {
  return this.name + 'is' + this.age + 'years old';
}

var studentA = new Student('xiaomi', 98, 18);
console.log(studentA.toString());
```
通过上面代码，单个的toString()实例就可以被所有的Student对象所共享。

