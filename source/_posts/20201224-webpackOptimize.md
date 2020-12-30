---
title: webpack优化相关
tags: ["工作"]
categories: 工作
---

webpack优化相关知识记录。

<!--more-->

### 1.辅助工具 ：打包时间、打包分析 

* `speed-measure-webpack-plugin` 安装后可以查看打包耗时。可以看到每个包打包用了多长时间。

* `webpack-bundle-analyzer` 查看打包后每个包的大小。

```javascript
// 安装
cnpm install -D speed-measure-webpack-plugin
cnpm install -D webpack-bundle-analyzer
// 使用
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin')
const smp = new SpeedMeasurePlugin()
const webpack = require('webpack')
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
module.exports = {
  publicPath: './',
  productionSourceMap: isDev,
  pages: {},
  chainWebpack: config => {},
  configureWebpack: smp.wrap({
    plugins: [
      new BundleAnalyzerPlugin(),
      new webpack.DllReferencePlugin({
        context: process.cwd(),
        manifest: require('./public/vendor/vant-manifest.json')
      }),
      new webpack.DllReferencePlugin({
        context: process.cwd(),
        manifest: require('./public/vendor/vendor-manifest.json')
      })
    ]
  }),
  devServer: {}
}

```

### 2.代码分割

#### 2.1SplitChunks 

* `SplitChunks ` 集成在webpack中，顾名思义，就是用来手动将包抽离出来，以达到减小打包大小的方法。

SplitChunks 插件配置选项：

- chunks 选项，决定要提取那些模块。

  - 默认是 async：只提取异步加载的模块出来打包到一个文件中。
  - 异步加载的模块：通过 import('xxx')或 require(['xxx'],() =>{})加载的模块。
  - initial：提取同步加载和异步加载模块，如果 xxx 在项目中异步加载了，也同步加载了，那么 xxx 这个模块会被提取两次，分别打包到不同的文件中。

    - 同步加载的模块：通过 import xxx 或 require('xxx')加载的模块。

  - all：不管异步加载还是同步加载的模块都提取出来，打包到一个文件中。

- minSize 选项：规定被提取的模块在压缩前的大小最小值，单位为字节，默认为 30000，只有超过了 30000 字节才会被提取。
- maxSize 选项：把提取出来的模块打包生成的文件大小不能超过 maxSize 值，如果超过了，要对其进行分割并打包生成新的文件。单位为字节，默认为 0，表示不限制大小。
- minChunks 选项：表示要被提取的模块最小被引用次数，引用次数超过或等于 minChunks 值，才能被提取。
- maxAsyncRequests 选项：最大的按需(异步)加载次数，默认为 6。
- maxInitialRequests 选项：打包后的入口文件加载时，还能同时加载 js 文件的数量（包括入口文件），默认为 4。
- 先说一下优先级 maxInitialRequests / maxAsyncRequests < maxSize< minSize。
  automaticNameDelimiter 选项：打包生成的 js 文件名的分割符，默认为~。
  name 选项：打包生成 js 文件的名称。
- cacheGroups 选项，核心重点，配置提取模块的方案。里面每一项代表一个提取模块的方案。下面是 cacheGroups 每项中特有的选项，其余选项和外面一致，若 cacheGroups 每项中有，就按配置的，没有就使用外面配置的。
  - test 选项：用来匹配要提取的模块的资源路径或名称。值是正则或函数。
    priority 选项：方案的优先级，值越大表示提取模块时优先采用此方案。默认值为 0。
  - reuseExistingChunk 选项：true/false。为 true 时，如果当前要提取的模块，在已经在打包生成的 js 文件中存在，则将重用该模块，而不是把当前要提取的模块打包生成新的 js 文件。
  - enforce 选项：true/false。为 true 时，忽略 minSize，minChunks，maxAsyncRequests 和 maxInitialRequests 外面选项

核心嘛，就是配置 `cacheGroups` 来根据自己的需求来分割打包代码块。

vuecli3 中 splitChunk 的默认配置

```javascript
module.exports = {
  configureWebpack: (config) => {
    return {
      optimization: {
        splitChunks: {
          chunks: "async",
          minSize: 30000,
          maxSize: 0,
          minChunks: 1,
          maxAsyncRequests: 6,
          maxInitialRequests: 4,
          automaticNameDelimiter: "~",
          cacheGroups: {
            vendors: {
              name: `chunk-vendors`,
              test: /[\\/]node_modules[\\/]/,
              priority: -10,
              chunks: "initial",
            },
            common: {
              name: `chunk-common`,
              minChunks: 2,
              priority: -20,
              chunks: "initial",
              reuseExistingChunk: true,
            },
          },
        },
      },
    };
  },
};
```

贴一个自己项目的实验结果。

项目结构

![项目结构](https://s3.ax1x.com/2020/12/16/rlszlt.png)

按照默认配置打包后的可视化解析图：

![before](https://s3.ax1x.com/2020/12/16/rlyQ7F.md.png)

配置之后的代码：

```javascript
// vue.config.js
configureWebpack: (config) => {
  return {
    optimization: {
      splitChunks: {
        chunks: "async",
        minSize: 30000,
        maxSize: 0,
        minChunks: 1,
        maxAsyncRequests: 6,
        maxInitialRequests: 4,
        automaticNameDelimiter: "~",
        cacheGroups: {
          vendors: {
            name: "chunk-vendors",
            test: /[\\/]node_modules[\\/]/,
            priority: -10,
            chunks: "all",
          },
          mixIn: {
            chunks: "all",
            name: "mixIn",
            test: path.resolve(__dirname, "./src/views/medicineType/typeMixin"),
            priority: 0,
          },
        },
      },
    },
    plugins: [new BundleAnalyzerPlugin()],
  };
};
```

配置之后打包完的可视化：

![after](https://s3.ax1x.com/2020/12/16/rly73n.md.png)

可以看到变小了 20 多 K，并且把 mixin 给抽离出来单独打包了。

### 2.2  `DllPlugin`   `DllReferencePlugin`

* `DllPlugin` `DllReferencePlugin`  在使用webpack进行打包时候，对于依赖的第三方库，比如vue，axios，vuex等这些不会修改的依赖，我们可以让它和我们自己编写的代码分开打包，这样做的好处是每次更改我本地代码的文件的时候，webpack只需要打包我项目本身的文件代码，而不会再去编译第三方库，那么第三方库在第一次打包的时候只打包一次，以后只要我们不升级第三方包的时候，那么webpack就不会对这些库去打包，这样的可以快速的提高打包的速度。因此为了解决这个问题，DllPlugin 和 DllReferencePlugin插件就产生了。
  * `DllPlugin`  用来将dll抽离出来 
  * `DllReferencePlugin`  用来告诉webpack这些东西我已经抽离出来啦，你打包时候不用打啦。

```javascript
// 控制台
cnpm install webpack -g // 安装webpack
cnpm install webpack-cli -g // 安装webpack-cli
cnpm install -D clean-webpack-plugin // 安装clean-webpack-plugin

webpack -v // 查看安装是否成功

// issuser@ISS MINGW64 /d/work/gaokeappointment (Study-webpack)
// $ webpack -v
// webpack-cli 4.2.0
// webpack 5.11.0



// package.json
{
  "name": "gaokeappointment",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "serve:prod": "vue-cli-service serve --mode production",
    "build": "vue-cli-service build",
    "build:dev": "vue-cli-service build --mode development",
    "lint": "vue-cli-service lint",
    // 新加这行
    "dll": "webpack --progress --config webpack.dll.config.js"
  },
  "dependencies": {}
}



// webpack.dll.js
const path = require('path')
const webpack = require('webpack')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

// dll文件存放的目录
const dllPath = 'public/vendor'

module.exports = {
  entry: {
    // 需要提取的库文件
    vendor: ['vue', 'vue-router', 'vuex', 'axios'],
    vant: ['vant']
  },
  output: {
    path: path.join(__dirname, dllPath),
    filename: '[name].dll.js',
    // vendor.dll.js中暴露出的全局变量名
    // 保持与 webpack.DllPlugin 中名称一致
    library: '[name]_[hash]'
  },
  plugins: [
    // 清除之前的dll文件
    new CleanWebpackPlugin(),
    // 设置环境变量
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: 'production'
      }
    }),
    // manifest.json 描述动态链接库包含了哪些内容
    new webpack.DllPlugin({
      path: path.join(__dirname, dllPath, '[name]-manifest.json'),
      // 保持与 output.library 中名称一致
      name: '[name]_[hash]',
      context: process.cwd()
    })
  ]
}



// vue.config.js
const isDev = process.env.VUE_APP_ENV === 'development'
const path = require('path')
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin')
const smp = new SpeedMeasurePlugin()
const webpack = require('webpack')
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
module.exports = {
  // 生成模式使用cdn打包路径根据需要修改
  // 'https://f.taikang.com/static/'
  publicPath: './',
  productionSourceMap: isDev,
  pages: {},
  chainWebpack: config => {},
  configureWebpack: smp.wrap({
    plugins: [
      new BundleAnalyzerPlugin(),
      // 配置DllReferencePlugin
      new webpack.DllReferencePlugin({
        context: process.cwd(),
        manifest: require('./public/vendor/vant-manifest.json')
      }),
      new webpack.DllReferencePlugin({
        context: process.cwd(),
        manifest: require('./public/vendor/vendor-manifest.json')
      })
    ]
  }),
  devServer: {
    proxy: {
      '/': {
        target: 'https://sspuat.taikang.com',
        changeOrigin: true
      }
    }
  }
}



// 然后打包dll
npm run dll

// index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="./vendor/vendor.dll.js"></script>
    <script src="./vendor/vant.dll.js"></script>
  </head>
  <body>
    <noscript>
    </noscript>
    <div id="app"></div>
  </body>
</html>



// 然后就可以打包啦
npm run build
```

Before

![before](https://s3.ax1x.com/2020/12/25/rRteDP.png)

After 可以看到变小了200kb左右，`vant` 等组件被抽离出来了

![After](https://s3.ax1x.com/2020/12/25/rRtB8J.png)

时间变化：

Before|After
:--:|:--:
SMP  ⏱<br/>General output time took 8.35 secs|SMP  ⏱<br/>General output time took 7.75 secs
SMP  ⏱<br/>General output time took 8.55 secs|SMP  ⏱<br/>General output time took 7.79 secs
SMP  ⏱<br/>General output time took 8.33 secs|SMP  ⏱<br/>General output time took 7.78 secs

### 3.压缩

uglifyjs-webpack-plugin 传统老牌的压缩插件，缺点是不支持 es6。

terser-webpack-plugin 推荐使用，可以删除console等冗余的代码。

vue-cli3下 terser-webpack-plugin配置

```javascript
const isDev = process.env.VUE_APP_ENV === 'development'
const path = require('path')
module.exports = {
  publicPath: './',
  pages: {},
  chainWebpack: config => {
    if (isDev) return
    config.optimization.minimizer('terser').tap(args => {
      Object.assign(args[0].terserOptions.compress, {
        // pro mode drop console.log,but console.info or console.error will exist
        pure_funcs: ['console.log']
      })
      return args
    })
  },
  configureWebpack: smp.wrap({
    plugins: []
  }),
  devServer: {}
}

```

