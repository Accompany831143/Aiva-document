# Vuex

# Vuex

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。简单说就是用于组件间共享数据

### Vuex中，有五种基本的对象

- state：存储状态，也就是变量
- getters：对数据获取之前的再次编译，可以理解为state的计算属性。
- mutations：提交状态修改。也就是set、get中的set，唯一修改state的方式 ，不支持异步操作 。
- actions：Action 类似于 mutation，不同在于：
  - Action 提交的是 mutation，而不是直接变更状态。
  - Action 可以包含任意异步操作。
- modules：store的子模块，内容就相当于是store的一个实例 。

### 使用方式

使用vue-cli创建项目，并编辑store下的index.js文件

```javascript
import Vue from 'vue'		// 引入vue
import Vuex from 'vuex'		// 引入vuex

// 导出模块
export default new Vuex.Store({
  state: {
	name:'aiva',
	age:22
  },
  /* 
      getter 相当于计算属性
      getter中的方法默认有两个参数
        1.state 当前VueX对象中的状态对象
        2.getters 当前getters对象，用于将getters下的其他getter拿来用
  */
  getter:{
      lastAge(state) {
          return this.$store.state.age - 1;
      }，
      last(state,getters) {
          return state.name + getters.lastAge;
      }
  },
  /* 
      mutations 是更改state的唯一方式
      mutations中的方法必须是同步函数
      mutations中的方法默认有两个参数
        1.state 当前VueX对象中的状态对象
        2.Payload 该方法在被调用时传递的参数
  */
  mutations: {
  	changeName(state,name) {
        state.name = name;
  	}
  },
  /* 
      Action 类似于 mutation，不同在于：
        1.Action 提交的是 mutation，而不是直接变更状态。
        2.Action 可以包含任意异步操作。

      Actions中的方法有两个默认参数
        1.context 上下文(相当于箭头函数中的this)对象
        2.payload 挂载参数（该方法在被调用时传递的参数）
  */
  actions: {
  	editName(context,payload){
		context.commit('changeName',payload)
    }
  },
  // store的子模块，内容就相当于是store的一个实例 
  modules: {
  	childrenA:{
        state:{},
        getters:{},
        ....
    }
  }
})

```

默认vue-cli已配置好了vuex,没有配置的参考如下代码

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'		// 导入vuex的配置文件

Vue.config.productionTip = false

new Vue({
  router,
  store,	// 使用vuex
  render: h => h(App)
}).$mount('#app')
```


- 获取state

从store中获取
```javascript
    this.$store.state.name;
```
从mapState 辅助函数中获取
```html
    <template>
        <div>name:{{name}}</div>
    </template>

    <script>
        import {mapState} from 'vuex';
        export default {
            computed:{
                //将store.state中的属性映射到computed
                ...mapState(['name'])
            }
        }
    </script>
```
- 获取getters

从store中获取
```javascript
	this.$store.getters.lastAge;
```
从mapGetters 辅助函数中获取
```html
<template>
    <div>lastAge:{{lastAge}}</div>
</template>

<script>
    import {mapGetters} from 'vuex';
    export default {
        computed:{
            //将store.getters中的属性映射到computed
            ...mapState(['lastAge'])
        }
    }
</script>
```
- 调用 mutations

通过$store.commit调用mutation。
```javascript
    this.$store.commit('changeName','newName');
    // 对象风格
    store.commit({
      type: 'changeName',
      name: 'newName'
    })
```
从mapMutations 辅助函数中调用
```html
<template>
    <button @click="changeName('newName')">更改名字</button>
</template>

<script>
    import {mapMutations} from 'vuex';
    export default {
        methods:{
            //将store.mutations中的属性映射到methods
            ...mapState(['changeName'])
        }
    }
</script>
```
**注意：当使用对象风格的提交方式，整个对象都作为参数传给 mutations中的函数**

- 调用 actions

通过$store.dispatch调用actions。
```javascript
	this.$store.dispatch('editName','Jack');
```
从mapActions 辅助函数中调用
```html
<template>
    <button @click="$store.dispatch('editName','Jack')">更改为Jack</button>
</template>

<script>
    import {mapActions} from 'vuex';
    export default {
        methods:{
            //将store.mapActions中的属性映射到methods
            ...mapState(['editName'])
        }
    }
</script>
```