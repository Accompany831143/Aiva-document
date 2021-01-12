# React组件间常见的通信方式

使用React开发时，经常出现组件互相传递数据，本文介绍一下组件间常见的通信方式。

### 父组件向子组件通信

最常见也是最简单的通信方式

父组件通过props属性向子组件传值，子组件通过props获取传递的值


父组件
```javascript
import Child from "./child"

function default(){
    let str = 'Hello World!'
    return (
        <Child msg={str}></Child>
    )
}
```

子组件

```javascript
   export default class Child extends Component {
    constructor(props) {
        super(props)
    }

    render() {
        return (<h1>{this.props.msg}</h1>)
    }
}
```

### 子组件向父组件通信

父组件将函数作为props属性的值传递给子组件，子组件通过调用该函数来完成通信

父组件

```javascript
import Child from "./child"
function Parent(){
    function getProp(val) {
        console.log(val)
    }
    return (
        <Child get={getProp}></Child>
    )
}
```

子组件

```javascript
export default class Child extends Component {
    constructor(props) {
        super(props)
        this.state = {
            msg:'children'
        }
    }
        
    render() {
        return (
        <button onClick={() => {this.props.get(this.state.msg)}}>
        	给父组件传值
        </button>
        )
    }
}
```


### 跨级组件间通信

嵌套的组件过多时，底层组件如果想要获取顶层组件的数据，顶层组件可以逐级向下传递props来让底层组件获取数据，但这样如果组件嵌套过多就太麻烦了。我们可以使用**context对象**的方式来进行通信。

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。

> 在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。


- 概念

**Provider(生产者)**: 用于生产共享数据的地方。

```javascript
<Provider value={/*需要传递的数据*/}>
    /*渲染内容*/
</Provider>
```

**Consumer(消费者)**:  Consumer需要嵌套在生产者下面，通过回调函数的方式拿到共享的数据。

```javascript
<Consumer>
  {value => /*根据传递的数据来渲染内容*/}
</Consumer>
```

**一个简单的应用

父组件

```javascript
import React from 'react'

// 引入子组件
import Child from './Child.js'

// 创建一个上下文的容器(组件)，设置一个默认数据
export const {Provider,Consumer} = React.createContext('pink')

export default class Context extends React.Component {
	constructor(props) {
		super(props)
		this.state = {
			color:{
				value:'red',
				setValue:(val) => {
					this.setState({
						color:{
							value:val,
							setValue:this.state.color.setValue
						}
					})
				}
			},
		}
	}
	
	render() {
		let color = this.state.color
		return (
			<div>
				 // Provider共享数据的容器 value是需要传递的数据
				<Provider value={color}>
					<p>content</p>
					<Child />
				</Provider>
			</div>
		)
	}
}
```

子组件

```javascript
import React from 'react'

// 引入父组件的Consumer
import {Consumer} from './index.js'

import {Button} from 'antd'

// 引入子组件
import Son from './Son.js'

export default class Child extends React.Component {
	constructor(props) {
		super(props)
		this.state = {}
	}
	
	render() {
		return (
			<div>
			/* 通过Consumer来获取Provider传递的数据 */
				<Consumer>
					{
						(color) => 
							<div>
							/* 获取数据 */
								<p style={{color:color.value}}>Child:{color.value}</p>				
								/* 调用setValue函数 */
								<Button onClick={() => {color.setValue('blue')}}>蓝色</Button>
								<Son />
							</div>
					}
				
				</Consumer>
			</div>
		)
	}
}
```


孙组件

```javascript
import React from 'react'

// 引入父组件的Consumer
import {Consumer} from './index.js'

import {Button} from 'antd'
export default class Son extends React.Component {
	constructor(props) {
		super(props)
	}
	
	render() {
		return (
			<div>
			/* 通过Consumer来获取Provider传递的数据 */
				<Consumer>
					{
						(color) => 
							<div>
							/* 获取数据 */
								<p style={{color:color.value}}>Son:{color.value}</p>
								
								<Button onClick={() => {color.setValue('green')}}>绿色</Button>
							</div>

					}
				</Consumer>
			</div>
		)
	}
}
```


以上就是React中组件常见的通信方式

End