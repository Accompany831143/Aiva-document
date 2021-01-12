# Vue中的路由 Router

Vue中的路由 Router

Vue Router 是 Vue.js 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。

单页应用SPA

所谓单页应用，指的是在一个页面上集成多种功能，甚至整个系统就只有一个页面，所有的业务功能都是它的子模块，通过特定的方式挂接到主界面上。它是AJAX技术的进一步升华，把AJAX的无刷新机制发挥到极致，因此能造就与桌面程序媲美的流畅用户体

单页应用SPA 实现

页面切换：浏览器地址栏哈希值控制隐藏显示div

页面加载：通过ajax动态获取

路由视图结构

存放页面 <router-view>

切换路由 <router-link>

路由配置

    {
        path: '/about',  // 路由连接地址 和router-link to值对应
        name: 'Home', // 路由的名称 
        component: Home 
        // 路由页面使用的组件被填充的router-view里面的组件              
    },

路由参数配置

1. 动态路径参数

    // 配置
    {
        path:“/produce/:id”
        name:"produce"
        component:Produce
    }
    // 跳转链接
    <router-link to="/produce/123">Go</router-link>
    /*
    :id就是给路由配置的参数	
    当匹配到一个路由时，参数值会被设置到 this.$route.params
    使用$route.params.id获取参数
    */

1. 查询字符串参数

    <router-link to="/produce?id=123">Go</router-link>
    <!-- 使用$route.query.id获取参数  -->

router和route区别

1. $route
   - Object类型对象
   - 内部有params  第一种路由传参  /detail/:id   /detail/101   this.$route.params.id
   - query  路由传参  第二种 ?redirect=carts    this.$route.query.redirect
   - path 路由路径
   - meta 路由携带数据  { meta:{ auth:true,name:aiva }  }
2. $router 一个VUE的路由对象
   - this.$router.push(“目标路由”)
   - this.$router.go(-1) 回退

路由导航

- router.push
  push方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL
  该方法的参数可以是一个字符串路径，或者一个描述地址的对象

    // 字符串
    router.push('home')
  
    // 对象
    router.push({ path: 'home' })
  
    // 命名的路由
    router.push({ name: 'user', params: { userId: '123' }})
  
    // 带查询参数，变成 /register?plan=private
    router.push({ path: 'register', query: { plan: 'private' }})

- router.replace
  router.replace跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

    this.$router.replace("home")

- router.go
  这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。

    	this.$router.go(1)	// 前进1步
    	this.$router.go(-1)	// 后退1步

跳转

    <router-link to=/home">首页</router-link> 
    <router-link :to="{ path: 'home' }">Home</router-link> 
    <router-link :to="{ name: 'produce', params: { id: 123 }}">
    产品123
    </router-link>

重定向

    { path: '/home', redirect: '/' }

别名

    	{ path: '/a', component: A, alias: '/b' }

404

    {path: '*'// 会匹配所有路径}

路由高亮

.router-link-active

全局配置 <router-link> 默认的激活的 class

.router-link-exact-active

全局配置当链接被精确匹配的时候应该激活的 class

路由嵌套

路由可以嵌套，一个被渲染路由组件同样可以包含自己的嵌套 <router-view>

- 要在嵌套的出口中渲染组件，需要在 VueRouter 的参数中使用 children 配置

    const router = new VueRouter({
      routes: [
        { path: '/user/:id', component: User,
          children: [
            {
              // 当 /user/:id/profile 匹配成功，
              // UserProfile 会被渲染在 User 的 <router-view> 中
              path: 'profile',
              component: UserProfile
            },
            {
              // 当 /user/:id/posts 匹配成功
              // UserPosts 会被渲染在 User 的 <router-view> 中
              path: 'posts',
              component: UserPosts
            }
          ]
        }
      ]
    })

要注意，以 / 开头的嵌套路径会被当作根路径。嵌套的路由不需要使用'/'

路由权限控制

beforeEach（） 全局前置守卫 进入前调用

afterEach（）    全局后置钩子 进入后调用

每个守卫 三个参数：

- to: Route: 即将要进入的目标 路由对象
- from: Route: 当前导航正要离开的路由
- next:  一定要调用该方法来路由才会切换。
- next() 直接进入下个路由
- next(false) 路由回到from页面
- next('/') 路由跳转到首页

路由元信息

给路由配置额外的信息

    { path:"/cart",
        ...    
        meta:{requireAuth:true}
    }
    // $route.meta.requireAuth 获取

组件内部守卫

- beforeRouteEnter  进入前 （没有this）
- beforeRouteUpdate  参数更新是
- beforeRouteLeave   路由离开前

    const Foo = {
      template: `...`,
      beforeRouteEnter (to, from, next) {
        // 在渲染该组件的对应路由被 confirm 前调用
        // 不！能！获取组件实例 `this`
        // 因为当守卫执行前，组件实例还没被创建
      },
      beforeRouteUpdate (to, from, next) {
        // 在当前路由改变，但是该组件被复用时调用
        // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
        // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
        // 可以访问组件实例 `this`
      },
      beforeRouteLeave (to, from, next) {
        // 导航离开该组件的对应路由时调用
        // 可以访问组件实例 `this`
      }
    }

路由过渡

    <transition>
      <router-view></router-view>
    </transition>

组件缓存

    <keep-alive>
      <router-view  v-if="$route.meta.keep" />
    </keep-alive>
      <router-view v-if="!$route.meta.keep" />

路由懒加载

    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')