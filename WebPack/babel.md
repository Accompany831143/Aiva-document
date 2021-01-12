# webpack webpack配置babel


> babel是一个javascript编译器，是前端开发中的一个利器。它突破了浏览器实现es标准的限制，使我们在开发中可以使用最新的javascript语法。
通过构建和babel，可以使用最新js语法进行开发，最后自动编译成用于浏览器或node环境的代码。


### 基本使用
**本文基于webpack4.x**

- 安装babel-loader, @babel/core, @babel/preset-env

```bash
	npm install -D babel-loader @babel/core @babel/preset-env
```

- 在webpack.config.js中配置

```javascript
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
      }
    }
  ]
}
```

- 新建babael.config.js文件，编辑内容

```javascript
module.exports = {
    presets: ['@babel/preset-env']
}
```

### 避免全局污染

Babel 对一些公共方法使用了非常小的辅助代码，默认情况下会被添加到每一个需要它的文件中。
引入 Babel runtime 作为一个独立模块，来避免重复引入。

- 安装 

```bash
	npm install -D @babel/plugin-transform-runtime @babel/runtime
```

- 使用 

编辑babael.config.js文件
```javascript
module.exports = {
    presets: ['@babel/preset-env'],
    plugins: ['@babel/plugin-transform-runtime']
}
```

更多内容请查看官方[文档](https://webpack.docschina.org/loaders/babel-loader/)