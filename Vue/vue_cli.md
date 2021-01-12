# 使用Vue-cli创建项目

>本文介绍一下 怎么用Vue脚手架创建项目

*本篇文章基于Vue-cli 4.2.2*

#### 第一步 安装Node.js 
	Node.js自带NPM 包管理工具
#### 第二步 安装VUE脚手架

```bash
	npm install -g @vue/cli 
```

#### 第三步 新建项目

```bash
	vue create project-name
```

1. 选择一个预设（预先设定好，比如上次创建工程使用参数）  使用上下键选择，回车确定
```bash
Please pick a preset: (Use arrow keys)
default (babel, eslint) 	// 默认设置
Manually select features 	// 手动根据需要选择
```
选择  Manually select features 手动配置

2. 选择工程所需插件
```bash
Check the features needed for your project:
(Press <space> to select, <a> to toggle all, <i> to invert selection)
```

*空格选择  a键选择所有  i键反选*

注：

- **Babel**		将ES6代码转ES5 （部分浏览器不支持部分ES6代码）

	 **TypeScript**	JavaScript的超集 （最终需要编译成js）

	 **Progressive Web APP**	移动端支持

	 **Router**	 Vue路由支持

	 **VueX**		Vue的状态管理模式 （数据管理中心）

	 **CSS Pre-processors**	（CSS预处理、可以转为Less、Sass为CSS）

	 **linter/Formatter**	 语法检查格式化工具

	 **Unit Testing** 	 单元测试

	 **E2E Testing**	 端对端测试

根据项目需求安装插件,这里选择了Babel,,Router,CSS Pre-procrssors,VueX

3. 是否对路由使用history模式
```hash
Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)
```

vue中普通模式路由后带有#  只切换#后内容
history模式下不带有# 

回车	yes


4. 选择一个css预处理器
```bash
Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
Sass/SCSS (with dart-sass)
Sass/SCSS (with node-sass)
* Less
Stylus 
```

此处选择 Less 

5. 项目依赖如何保持
```hash
Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
In dedicated config files	// 专用的文件夹
* In package.json 	// 保存至package.json
```
建议选择 In package.json

6.是否保存这次配置？	
```bash
	Save this as a preset for future projects? (y/N)
```
选择y	下次使用vue脚手架创建项目时可直接选择本次的配置

回车创建项目 等待插件安装