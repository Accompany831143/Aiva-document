# Angular2.x 入门

## Angular是什么？

> Angular（读音[‘æŋgjʊlə]）是一套用于构建用户界面的 JavaScript 框架。由 Google 开发和维护，主要被用来开发单页面应用程序。

## 特性

- MVVM
- 组件化
- 模块化
- 指令
- 服务
- 依赖注入
- TypeScript


## Angular2.x与Angular1.x 的区别

Angular2.x与Angular1.x 的区别类似 Java 和 JavaScript 或者说是雷锋与雷峰塔的区别，所以在学习Angular2.x时大家需要做好重新学习一门语言的心里准备。

## 使用Angular CLI 快速构建项目

*安装命令行工具前请确保已安装node.js，这里不过多赘述。*

#### 全局安装Angular命令行工具

```bash
	npm install -g @angular/cli
```

#### 初始化项目

```bash
	ng new <项目名称>
```

#### 启动项目

在项目根目录下运行

```bash
	ng server
```
浏览器输入 127.0.0.1:4200 即可在浏览器打开项目

#### 项目目录结构

```
——
	── e2e 端对端测试
	── node_modules 项目依赖
	── src 项目源码
		—— app 应用的组件和模块
			—— app.component.css 组件样式文件
			—— app.component.html 组件模板文件
			—— app.component.spec.ts 组件单元测试文件
			—— app.component.ts 组件脚本文件
			—— app.module.ts 组件数据模型定义文件
		—— assets 资源文件目录
		—— enviroments 环境配置目录
		—— index.html 主页面
		—— main.ts 脚本入口文件
		—— polyfills.ts 检测兼容的文件信息
		—— styles.css 全局的样式文件
		—— test.ts 单元测试入口文件
	── .angular-cli.json AngularCLI脚手架配置文件
	── .editorconfig 针对编辑器的代码风格约束
	── .gitignore Git仓库忽略配置项
	── karma.conf.js 给karma用的测试配置文件
	── package.json 项目包说明文件
	── protractor.conf.js 端到端测试配置文件
	── README.md 项目介绍
	── tsconfig.json TypeScript配置文件
	── tslint.json TypeScript代码风格校验工具配置文件（类似于 eslint）
```

## 基本使用

### 模板语法

#### 显示数据

组件模板

```html
	<p>{{ msg }}</p>
```

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      msg = 'Hello world!';
    }
```

#### 模板表达式

```html
    <p>1 + 1 = \{\{ 1 + 1 \}\}</p>
    <p>\{\{ num + 1 \}\}</p>
    <p>\{\{ isDone ? '完了' : '没完' \}\}</p>
    <p>\{\{ title.split('').reverse().join('') \}\}</p>
    <p [title]="title.split('').reverse().join('')">\{\{ title \}\}</p>
```


#### 显示HTML

组件模板

```html
	<p [innerHTML]="code"></p>
```

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      code = '<span>标题<span>';
    }
```

#### 模板引用变量

模板引用变量 通常用来引用模板中的某个DOM元素，它还可以引用Angular组件或指令或Web Component。

使用井号 (#) 来声明引用变量。 #phone的意思就是声明一个名叫phone的变量来引用input元素。

我们可以在模板中的任何地方引用模板引用变量。 比如声明在input上的phone变量就是在模板另一侧的button上使用的。

```html
    <input #phone placeholder="phone number">
    <button (click)="callPhone(phone.value)">Call</button>
```

**模板引用变量使用注意**

- 模板引用变量的作用范围是 整个模板 。 不要在同一个模板中多次定义同一个变量名，否则它在运行期间的值是无法确定的。

- 如果真的出现了重名，Angular 会按照以下优先级来进行处理,模板局部变量 > 指令中的同名变量 > 组件中的同名属性

- 我们也可以用ref-前缀代替#。


#### 条件渲染

```html
	<!-- 如果flag为true时渲染，否则渲染myelse -->
	<p *ngIf="flag else myelse">条件为真</p>
	<ng-template #myelse>条件为假</ng-template>
```

#### 列表渲染

```html
	<!-- 对hero进行遍历 -->
    <ul>
        <li *ngFor="let hero of heroes">\{\{ hero \}\}</li>
    </ul>
    
    <!-- 获取 index 索引 -->
    <div *ngFor="let hero of heroes; let i=index">\{\{i + 1\}\} - \{\{hero\}\}</div>
```

#### 事件绑定

组件模板

```html
	<p>\{\{count\}\}</p>
	<button (click)="add()">Save</button>
```
*注意是有小括号的*

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
    	count:0;
    	
        add() {
          this.count = this.count + 1;
        }
    }
```

#### 属性绑定

组件模板

```html
	<img [src]="imgUrl" alt="" />
	或
	<img bind-src="imgUrl" alt="" />
	
	<a href="#" [title]="msg">{{msg}}</a>
```

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      imgUrl = 'http://xxxxxxx.jpg';
      msg:'我是一段文字'
    }
```

#### class绑定

组件模板

```html
    <style>
    .pink {color:pink;}
    </style>


    <!-- class为脚本文件中pink得值 -->
    <p [class]="pink">class绑定</p>

    <!-- 如果flag为true,则添加pink这个类 -->
    <p [ngClass]="{'pink':flag}">class绑定</p>

    <!-- 如果flag为true,则添加pink这个类，这种方式只能添加一个类 -->
    <p [class.pink]="flag">class绑定</p>
```

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      pink:'pink';
      flag:true;
    }
```

#### style绑定

组件模板

```html
    <p [style.color]="'red'">style绑定</p>
    <p [ngStyle]="{color:'red','font-size':'50px'}">style绑定</p>
    <p [ngStyle]="myStyle">style绑定</p>
```

组件脚本

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
      myStyle:{color:'red','font-size':'50px'};
    }
```

#### 数据双向绑定

导入 FormsModule 并把它添加到 Angular 模块的 imports 列表中

修改app.module.ts

```javascript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import {FormsModule} from '@angular/forms'  // 添加

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        FormsModule			// 添加
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
```

组件模板

```html
	<p>\{\{ message \}\}</p>
	<!-- 将input的value绑定到message -->
	<input type="text" [(ngModel)]="message">
```


#### 过滤器

在 Angular 中，过滤器也叫管道。它最重要的作用就是用来格式化数据的输出。

```html
	<!-- 使用内置过滤器date -->
	<h1>\{\{currentTime | date:'yyyy-MM-dd HH:mm:ss'\}\}</h1>
```

```javascript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.css']
    })
    export class AppComponent {
		currentTime:new Date()
	}

```

更多过滤器请参考[中文文档](https://angular.cn/guide/pipes)


End