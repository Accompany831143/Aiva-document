# React基础

## 介绍

React 是一个用于构建用户界面的 JAVASCRIPT 库。
React 主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。
React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。
React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。

## 特点

- 声明式设计 −React采用声明范式，可以轻松描述应用。

- 高效 −React通过对DOM的模拟，最大限度地减少与DOM的交互。

- 灵活 −React可以与已知的库或框架很好地配合。

- JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。

- 组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。

- 单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。


## 使用

### 元素渲染

1. 创建一个元素
2. 渲染到页面

```react
function default() {
	// 创建元素，必须只能有一个根元素
    const element = (
        <div>
            <h1>Hello, world!</h1>
            <h2>现在是 {new Date().toLocaleTimeString()}.</h2>
        </div>
    );
    // 利用ReactDOM.render方法渲染到页面
    return (
        element,
        document.getElementById('app')
    );
}

```

### 条件渲染
条件渲染一般使用三目运算符

```react
	function default(){
        let flag = false;
        return (
             <h1>{flag ? '正确' : '错误'}</h1>
        );
	}
```

### 列表渲染

jsx中的数组里面可以包含 html Dom结构，在jsx 数组会自动展开

```react
	function default(){
		let list  =  [<div key="0">react</div>,<div key="1">vue</div>,<div key="2">angular</div>]

        return (
        	{list}
        )
	}
```

我们可以使用 JavaScript 的 map() 方法来创建列表。

```react
	function default(){
		let list  =  [react,vue,angular]

		let arr = list.map((item,index)=>{return(<li key={index}>{item}</li>)})
        return (<ul>{arr}</ul>)
	}
```

### 事件绑定
1. React 事件绑定属性的命名采用驼峰式写法，而不是小写。
2. 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM 元素的写法)

```react
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('链接被点击');
  }
 
  return (
    <a href="javascript:;" onClick={handleClick}>
      点我
    </a>
  );
}
```
**传递参数**
注意改变事件响应函数中的this指向

react事件传参方式举例

```react
	<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
	<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### Props
React中组件传值使用props
state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变

子组件取值
```react
	// 函数
	function default(props){
		let title = props.msg

        return (<h1>{title}</h1>)
	}
	
	// 类
	export default class Child extends Component {
        constructor(props) {
            super(props)
        }
		
		render() {
            return (<h1>{this.props.msg}</h1>)
		}
}
```

父组件传值

```react
	import Child from "./child"
	function default(){
		let str = 'Hello World!'
        return (
        	<Child msg={str}></Child>
        )
	}
```

父组件取值

```react
	import Child from "./child"
	function default(){
		function getProp(val) {
            console.log(val)
		}
        return (
        	<Child get={getProp}></Child>
        )
	}
```

子组件传值
```react
	export default class Child extends Component {
        constructor(props) {
            super(props)
            this.state = {
                msg:'children'
            }
        }
		
		render() {
            return (<button onClick={() => {this.props.get(this.state.msg)}}>给父组件传值</button>)
		}
}
```


### state使用
React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

*其实就是vue里面的data*

定义：

```react
	export default class Child extends Component {
        constructor(props) {
            super(props)
            this.state = {
                msg: '内容内容'
            }
        }
    }
```

修改：

```react
import React, { Component } from "react"

export default class Child extends Component {
    constructor(props) {
        super(props)
        this.state = {
            msg: '小明'
        }
    }

    change() {
    	// 通过this.setState方法修改state的内容
        this.setState({
            msg:'大明'
        })
    }

    render() {
        return (
            <p>{this.state.msg}</p>
            <button onClick={this.change.bind(this)}>按钮</button>
        )
    }
}
```



### 表单绑定

```react
import React, { Component } from "react"

export default class Child extends Component {
    constructor(props) {
        super(props)
        this.state = {
            msg: '内容内容'
        }
    }

    change(e) {
        console.log(e.target.value)
        this.setState({
            msg:e.target.value
        })
    }

    render() {
        return (
            <div>
            	/* 将输入框内容绑定到state.msg 通过change事件进行双向绑定*/
                <h3>{this.state.msg ? this.state.msg : '没有内容'}</h3>
                <input type="text" value={this.state.msg} onChange={this.change.bind(this)} />
            </div>
        )
    }
}
```