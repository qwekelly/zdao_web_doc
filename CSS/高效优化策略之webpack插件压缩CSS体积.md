# CSS 优化策略之webpack插件压缩CSS体积

>对于CSS文件中包含的不必要的字符，例如注释、空白和缩进，我们可以在生产环境中将其删除，以达到减小文件大小的目的，这种技术也叫minification。而这些可以利用webpack构建工具进行实现。


## mini-css-extract-plugin
提取CSS

安装插件：
```javascript
npm install --save-dev mini-css-extract-plugin
```
webpack.config.js 配置
```javascript
 const MiniCssExtractPlugin = require('mini-css-extract-plugin')

// css,scss,sass,less
{
    test:/\.(sa|sc|c)ss$/,
    use: [
      process.env.NODE_ENV === 'development' ? 'style-loader': MiniCssExtractPlugin.loader,
      'css-loader',
      'sass-loader'
    ]
}

//最后对应环境下的plugins中
plugins: [
  new MiniCssExtractPlugin({
    filename: "[name].css"
  })
]
```
`MiniCssExtractPlugin 推荐只用于生产环境，因为该插件在开发环境下会导致HMR功能缺失，所以日常开发中，还是用style-loader。`
（原文描述：This plugin should be used only on `production` builds without `style-loader` in the loaders chain, especially if you want to have HMR in development.）

## optimize-css-assets-webpack-plugin
用于优化或者压缩CSS资源

安装相关插件：
```javascript
npm install --save-dev optimize-css-assets-webpack-plugin
npm install --save-dev cssnano
npm install --save-dev uglifyjs-webpack-plugin
```
这个插件接受以下参数：
+ `assetNameRegExp`：正则表达式，用于匹配需要优化或者压缩的资源名。默认值是 /\.css$/g
+ `cssProcessor`：用于压缩和优化CSS 的处理器，默认是 cssnano.这是一个函数，应该按照 cssnano.process 接口(接受一个CSS和options参数，返回一个Promise)
+ `canPrint`：表示插件能够在console中打印信息


webpack文档指出：最好设置 optimization.minimizer 项来缩小输出。

`
To minify the output, use a plugin like optimize-css-assets-webpack-plugin. Setting optimization.minimizer overrides the defaults provided by webpack.
`

如果单单使用这个配置，会出现只压缩CSS代码的问题，原来webpack4配置好的js压缩无效，这时就需要重新配置 UglifyJsPlugin 了。
UglifyJsPlugin 支持如下几个参数：
+ `cache`：Boolean/String,字符串即是缓存文件存放的路径
+ `parallel`：是否启用多线程并行运行提高编译速度
+ `uglifyOptions`：其他配置项

压缩配置：
```javascript
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
const CssNano = require('cssnano');

module.exports = {
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        test: /\.js(\?.*)?$/i,  //测试匹配文件
        cache: true,
        parallel: true,
        sourcMap: true,
        uglifyOptions: {
          output: {
            comments: false  // 输出删掉所有注释
          }，
          compress: {
            warning: false, // 插件在进行删除一些无用的代码时不提示警告
            drop_console: true // 过滤console,即使写了console,线上也不显示
          }
        }
      }),
      new OptimizeCssAssetsPlugin({
        assetNameRegExp: /\.(sa|sc|c)ss$/g,
        cssProcessor: CssNano,
        cssProcessorOptions: {
          safe: true,
          discardComments: { removeAll: true }, //对CSS文件中注释的处理：移除注释
          normalizeUnicode: false // 建议false,否则在使用unicode-range的时候会产生乱码
        },
        canPrint: true
      }),
    ],
  },
  module: {
    rules: [
      {
        test:/\.(sa|sc|c)ss$/,
        use: [
          process.env.NODE_ENV === 'development' ? 'style-loader': MiniCssExtractPlugin.loader,
          'css-loader',
          'sass-loader'
        ]
      }
    ],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: "[name].css"
    }),
  ]
};
```

## 参考

https://web.dev/minify-css/

https://webpack.js.org/plugins/mini-css-extract-plugin/

https://www.webpackjs.com/plugins/uglifyjs-webpack-plugin/