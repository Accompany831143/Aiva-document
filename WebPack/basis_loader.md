# webpack 基本配置（包含插件，loader）


> 本文基于我的另一篇文章[webpack基本配置](https://www.jianshu.com/p/fea85910c212)的基础上新增了一些常用插件和loader的安装


### 安装
**安装之前请确保电脑已经安装了node.js**

新建目录
```bash
	npm init 	// 项目初始化
	npm install webpack webpack-cli -D   	// 安装webpack和webpack命令行工具
```
### 配置
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

![图片](https://upload-images.jianshu.io/upload_images/20584634-3eac52bca75c1bad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 使用webpack-dev-server
webpack-dev-server 是一个简单的 web 服务器，并且能够实时重新加载。

- 安装 

```bash
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
        contentBase:'./views' 	// 设置views目录下的文件为服务器根目录
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
### 使用webpack插件
插件是 webpack 的支柱功能。webpack 自身也是构建于，你在 webpack 配置中用到的相同的插件系统之上！
插件目的在于解决 loader 无法实现的其他事。

#### html-webpack-plugin
官网是这么说的

HtmlWebpackPlugin简化了HTML文件的创建，以便为你的webpack包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个HTML文件，使用lodash模板提供你自己的模板，或使用你自己的loader。

**解释**
html-webpack-plugin 插件是用于编译 Webpack 项目中的 html 类型的文件，如果直接将 html 文件置于 ./src 目录中，用 Webpack 打包时是不会编译到生产环境中的，html-wepack-plugin就解决了这个问题，可以将js动态的插入到html当中

- 安装
```bash
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

#### mini-css-extract-plugin
该插件用于把css单独抽出为样式文件

- 安装

```bash
	npm install mini-css-extract-plugin -D
```

- 配置


打开webpack.config.js文件，编辑内容
```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
......
    plugins:[
        new htmlWebpackPlugin({
            filename:'index.html',
            template:'./views/index.html'
        }),
        new MiniCssExtractPlugin({
        filename: 'style.css',
        //所有的css文件抽出为style.css
    	}),
    ],
    module: {
    	rules: [
            { test: /\.css$/, use: [MiniCssExtractPlugin.loader, 'css-loader'] },
            { test: /\.less$/, use: [MiniCssExtractPlugin.loader, 'css-loader', 'less-loader'] },
            { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' },
    	]
	}
```

#### optimize-css-assets-webpack-plugin && uglifyjs-webpack-plugin
optimize-css-assets-webpack-plugin 用来优化css
uglifyjs-webpack-plugin 用来优化js

- 安装

```bash
	cnpm install optimize-css-assets-webpack-plugin uglifyjs-webpack-plugin -D
```

- 配置

编辑weboack.config.js 配置文件

```javascript
// 引入插件
const OptimizeCss= require('optimize-css-assets-webpack-plugin');
const UglifyjsWebpackPlugin = require("uglifyjs-webpack-plugin");

module.exports = {
    ......
    optimization: {
		minimizer: [
            new OptimizeCss({}), 
            new UglifyjsWebpackPlugin()
		]
	},
}
```

### 使用loader
webpack 可以使用 loader 来预处理文件。这允许你打包除 JavaScript 之外的任何静态资源。你可以使用 Node.js 来很简单地编写自己的 loader

#### css-loader
css-loader用来分析css引用关系
style-loader用来生成样式内容插入到header里面


- 安装

css-loader依赖style-loader
```bash
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

#### url-loader
file-loader 将文件发送到输出文件夹，并返回（相对）URL
url-loader 像 file loader 一样工作，但如果文件小于限制，可以返回 data URL

- 安装

url-loader依赖file-loader
```bash
	npm install url-loader file-loader -D
```

- 配置

打开weboack.config.js 编辑内容
```javascript
    module: {
        rules: [
            { test: /\.css$/, use: ['style-loader','css-loader'] },
            { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' },
            { test: /\.(ttf|eot|svg|woff|woff2)$/, use: ['url-loader'] },
			{ test: /\.(jpg|png|tmp|gif)$/, use: [
                {
                    loader:'url-loader',
                    options:{
                        limit:1024 * 20
                        // 单位为字节
                        // 低于指定的限制时，返回一个 DataURL。
                    },

                },
				'image-webpack-loader'
				// image-webpack-loader用于压缩图片
			] 
			},
        ]
    }
```

#### less-loader
less-loader 用来解析less语法，配置后可以在项目中使用less

- 安装

```bash
	npm install less-loader less -D;
```

- 配置
打开weboack.config.js 编辑内容
```javascript
    module: {
        rules: [
            { test: /\.css$/, use: ['style-loader','css-loader'] },
            { test: /\.less$/, use: ['style-loader', 'css-loader','less-loader'] },
            { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' }
        ]
    }
```
#### postcss-loader
postcss-loader 应该是 Webpack 配置中不可或缺的一个 CSS loader。它负责进一步处理 CSS 文件，比如添加浏览器前缀，压缩 CSS 等。

- 安装
autoprefixer用于添加浏览器前缀
```bash
	npm install postcss-loader autoprefixer -D;
```

- 配置

编辑weboack.config.js 配置文件
```javascript
	module: {
		rules: [
			{
				test: /\.css$/,
				use: [
					'style-loader', 
					'css-loader',
					{
						loader: 'postcss-loader',
						options: {
							plugins: [require('autoprefixer')]
						}
					}
				]

			}
		]
	}
```

在package.json中添加内容
```json
	"browserslist": [
    	"> 1%",
    	"last 2 versions"
  	]
```

