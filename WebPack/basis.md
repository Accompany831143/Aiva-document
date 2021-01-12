# webpack 基本配置

### webpack 基本配置

> webpack是每个前端开发者应该掌握的技能，这篇文章记录一下webpack的基本配置。
#### 安装
**安装之前请确保电脑已经安装了node.js**

- 新建目录
```
	npm init 	// 项目初始化
	npm install webpack webpack-cli -D   	// 安装webpack和webpack命令行工具
```
#### 配置
- 安装完成后新建目录结构
```
	 node_modules	// 项目依赖 （默认生成）
	 package.json	// 项目描述 （默认生成）
	 src				// 源码目录 （手动创建）
		index.js		// 入口js文件	
	 views				// 视图文件夹 （手动创建）
	 	index.html		// 入口html文件
	 webpack.config.js 	// webpack配置文件 （手动创建）
```
- 打开webpack.config.js，编辑内容
```javascript
	const path = require('path');	// 引入node里面内置的path模块
	module.expres = {
        entry:'./src/index.js',		// 入口文件位置
    	output:{
        	filename:'bundle.js',		// 打包完成后的文件名
        	path:path.join(__dirname,'dist')   // 打包完成后输出的位置
    	}
	}
```

- 配置项目脚本文件
打开package.json文件，新建项目脚本
```json
	{
        ......
        "scripts": {
            "test": "echo \"Error: no test specified\" && exit 1",
            "build":"webpack"		// 添加脚本
        },
        ......
	}
```
- 输入命令  npm run build，出现下列代码，就说明打包成功

![图片](https://upload-images.jianshu.io/upload_images/20584634-a7748ca8e05e3ec0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 使用webpack-dev-server
webpack-dev-server 是一个简单的 web 服务器，并且能够实时重新加载。
- 安装 
```
	npm install webpack-dev-server -D
```
- 打开webpack.config.js，添加webpack-dev-server配置
```javascript
	entry:'./src/index.js',
    output:{
        filename:'bundle.js',
        path:path.join(__dirname,'dist')
    },
    // wepack-dev-server 配置
    devServer:{
        port:8080,		// 配置端口为8080
        hot:true,		// 打开热更新
        open:true,		// 启动后打开浏览器
        contentBase:'./views' 	// 设置views目录下的文件为可访问文件
    }
```
- 配置项目脚本文件
打开package.json文件，新建项目脚本
```json
	{
        ......
        "scripts": {
            "test": "echo \"Error: no test specified\" && exit 1",
            "build":"webpack",
            "dev":"webpack-dev-server"		// 添加脚本
        },
        ......
	}
```
- 输入命令  npm run dev，出现下列代码，就说明配置成功
```
	i ｢wds｣: Project is running at http://localhost:8080/
    i ｢wds｣: webpack output is served from /
    i ｢wds｣: Content not from webpack is served from ./views
    i ｢wdm｣: wait until bundle finished: /
    i ｢wdm｣: Hash: a098c315bf6e497c3966
```
##### 使用html-webpack-plugin
官网是这么说的

HtmlWebpackPlugin简化了HTML文件的创建，以便为你的webpack包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个HTML文件，使用lodash模板提供你自己的模板，或使用你自己的loader。

**解释**
html-webpack-plugin 插件是用于编译 Webpack 项目中的 html 类型的文件，如果直接将 html 文件置于 ./src 目录中，用 Webpack 打包时是不会编译到生产环境中的，html-wepack-plugin就解决了这个问题
- 安装
```npm
	npm install html-webpack-plugin -D
```
- 配置
打开webpack.config.js文件，编辑内容
```javascript
const path = require('path');
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'bundle.js',
        path:path.join(__dirname,'dist')
    },
    devServer:{
        port:8080,
        hot:true,
        open:true,
        contentBase:'./views'
    },
    // 添加内容
    plugins:[
        new htmlWebpackPlugin({
            filename:'index.html',
            template:'./views/index.html'
        })
    ]
}
```

##### 使用loader
webpack 可以使用 loader 来预处理文件。这允许你打包除 JavaScript 之外的任何静态资源。你可以使用 Node.js 来很简单地编写自己的 loader
- 安装css-loader
	css-loader依赖style-loader
```
	npm install style-loader css-loader -D
```
- 配置
	打开weboack.config.js 编辑内容
```javascript
	plugins: [
        new htmlWebpackPlugin({
            filename: 'index.html',
            template: './views/index.html'
        })
    ],
    // 添加内容
    module: {
        rules: [
            {
                test: /\.css$/, use: ['style-loader','css-loader']
            }
        ]
    }
```

- 安装url-loader
	url-loader依赖file-loader
```
	npm install url-loader file-loader -D
```
- 配置
	打开weboack.config.js 编辑内容
```javascript
	plugins: [
        new htmlWebpackPlugin({
            filename: 'index.html',
            template: './views/index.html'
        })
    ],
    // 添加内容
    module: {
        rules: [
            {
                test: /\.css$/, use: ['style-loader','css-loader'],
            },
            { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' }
        ]
    }
```

更多配置请参考 [webpack 基本配置（包含插件，loader）](https://www.jianshu.com/p/7d49bee548d2)

配置babel请参考 [webpack配置babel](https://www.jianshu.com/p/187f17546c7b)
