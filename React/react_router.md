# React路由

### 引入

```javascript
	import { BrowserRouter as Router, Route, NavLink，Link } from "react-router-dom";
	
	// Router         总路由
    // Link           路由导航链接
    // Route          路由视图
    // NavLink	 	  选中的路由导航链接会包含一个active类
```

### 基本使用

```react
ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/about">关于</NavLink>
        </p>
        <Route path="/" exact component={Home} />
        <Route path="/about" component={About} />
    </Router>,
  document.getElementById('root')
);

function About() {

  return (<h3>关于我们xxxxxxxxxxxxxxxx</h3>)
}

function Home() {

  return (<h1>首页</h1>)
}
```

Route 的path属性为对应的地址，component属性为对应的路由组件

### 路由传参

```react
ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/detail/123">关于</NavLink>
        </p>
        <Route path="/" exact component={Home} />
        <Route path="/detail/:id" component={Detail} />
    </Router>,
  document.getElementById('root')
);

function Detail(props) {
  return (<h3>商品信息{props.match.params.id}</h3>)
}

function Home() {
  return (<h1>首页</h1>)
}

```

路由组件的props参数是一个对象，包含了history,location,match三个属性

- history包含了路由的跳转方法
- location包含了路由信息
- match包含了配置的地址和路由参数


### 路由嵌套

子路由可以嵌套路由

```react
ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/detail/123">详情</NavLink>
        </p>
        <Route path="/" exact component={Home} />
        <Route path="/detail/:id" component={Detail} />
    </Router>,
  document.getElementById('root')
);

// 这里通过ES6的结构赋值获取到了match
function Detail({match}) {
	return (
		<div>
			/* 嵌套路由 */
			<NavLink to={match.url + "/arg/" + match.params.id>参数</NavLink>
			|
			<NavLink to={match.url + "/comm"}>评论</NavLink>
			
			
			<Route path={match.url + "/arg/:id"} component={Arg} />
			<Route path={match.url + "/comm"} component={Comm} />
		</div>
	)
}

function Home() {

  return (<h1>首页</h1>)
}
```

### 重定向

需要导入 Redirect

```javascript
	import { BrowserRouter as Router, Route, NavLink,Redirect } from 'react-router-dom'
```

```react
ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/detail/123">详情</NavLink>
        </p>
        <Route path="/" exact component={Home} />
        <Route path="/detail/:id" component={Detail} />
    </Router>,
  document.getElementById('root')
);

// 这里通过ES6的结构赋值获取到了match
function Detail({match}) {
	return (
		<div>
			<NavLink to={match.url + "/arg/" + match.params.id>参数</NavLink>
			|
			<NavLink to={match.url + "/comm"}>评论</NavLink>
			
			
			<Route path={match.url + "/arg/:id"} component={Arg} />
			<Route path={match.url + "/comm"} component={Comm} />
			/* 重定向 */
			<Redirect from={match.url} to={match.url + "/comm"} />
		</div>
	)
}

function Home() {

  return (<h1>首页</h1>)
}
```


### 404页面

```react
import { BrowserRouter as Router, Route, NavLink,Redirect,Prompt,Switch } from 'react-router-dom'

ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/about">关于</NavLink>
        |
        <NavLink to="/asdasdasd">404</NavLink>
        </p>
        /* Switch 只显示一个路由 需要导入*/
		<Switch>
			<Route path="/" exact component={Home} />
			<Route path="/about" component={About} />
        	<Route component={NoFound} />
		</Switch>
    </Router>,
  document.getElementById('root')
);

function NoFound({history,location}) {
	return ( 
		<div>
			<h1>你访问的这个页面我找不到呀!</h1>
			<p>路径是{location.pathname}</p>
			<div className="nofound">
				<button onClick={() => {history.goBack()}}>后退</button>
				<button onClick={() => {history.push('/')}}>首页</button>
			</div>
		</div>
	)
}

function About() {

  return (<h3>关于我们xxxxxxxxxxxxxxxx</h3>)
}

function Home() {

  return (<h1>首页</h1>)
}


```

### Prompt  退出提示

```react
import { BrowserRouter as Router, Route, NavLink,Redirect,Prompt,Switch } from 'react-router-dom'

ReactDOM.render(
    <Router>
        <p className="nav">
        <NavLink to="/" exact>首页</NavLink>
        |
        <NavLink to="/about">关于</NavLink>
        </p>
        <Route path="/" exact component={Home} />
        <Route path="/about" component={About} />
    </Router>,
  document.getElementById('root')
);

function About() {

  return (
	<div>
		<h3>关于我们xxxxxxxxxxxxxxxx</h3>
		/* Prompt 离开时提示 需要导入*/
		<Prompt message="你真的要离开么？" />
	</div>
  )
}

function Home() {

  return (<h1>首页</h1>)
}
```

### 让非路由组件具有 路由props

WithRouter 让非路由组件具有 路由props

```react
	import React from 'react';
	import {withRouter} from 'react-router-dom'
	
	class Child extends React.Component {
		constructor(props) {
            super(props)
		}
		
		render() {
            return (
            	<div>
            		<button onClick={() => {this.props.history.goBack()}}>返回</button>
            	</div>
            )
		}
	}
	
	export default withRouter(Child);
```