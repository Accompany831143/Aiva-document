# 微信小程序开发入门篇

### 准备工作

1. 登录[微信公众平台](https://mp.weixin.qq.com/)注册开发者
2. 注册完成后登录，下载微信开发者工具

	在 `开发`  => `开发设置`里查看自己的APPID。*这是小程序的身份证号，不要告诉别人哈*

3. 启动微信开发者工具，就可以创建项目了。

### 目录结构：

1：pages 存放项目页面相关文件

![image](//upload-images.jianshu.io/upload_images/5640239-b6e071a3f3093ca4.png?imageMogr2/auto-orient/strip|imageView2/2/w/307/format/webp)

2：utils 里面包含一些公共的代码抽取的js文件，作为模块方便使用

![image](//upload-images.jianshu.io/upload_images/5640239-d19867a06ac74b5c.png?imageMogr2/auto-orient/strip|imageView2/2/w/305/format/webp)

3： 配置文件

![image](//upload-images.jianshu.io/upload_images/5640239-24725361773bb62f.png?imageMogr2/auto-orient/strip|imageView2/2/w/296/format/webp)


### 进入正题

#### 基本标签组件
<view></view>
<text></text>

#### 模板语法

- 条件渲染
```html
    <view wx:if="{{表达式}}">xxx</view>
    <view wx:elif="{{表达式}}">xxx</view>
    <view wx:else>xxx</view>
```

- 列表渲染

```html
	<view wx:for="{{list}}" wx:key="{{index}}">{{index}}--{{item}}</view>

    <text wx:for="{{list}}" wx:for-item="myitem" wx:for-index="myindex">
    	{{myindex}}~{{myitem}}
    </text>

```

- 模板 template

```html
<!-- 定义 -->
<template name="temp">
	<view>标题：{{name}}</view>
</template>

<!-- 调用 -->
<template is="temp" {{name:"han"}}>

<!-- 导入模板 -->
<import src = "xxx.wxml" />

```

- 模块引用

```html
<include src="xxx.wxml" />
```

##### 注意：WXML 提供 import 和 include 两种文件引用方式。

* import 有作用域的概念，不能多重引用
* include 就可以多重引用
* import是只引用<template/>模板，而include是copy除<template/>标签以外的全部代码


##### 事件

基本事件类型

`bindtap` 		单击
`bindinput` 	 表单发生改变

事件参数

```html
    <view bindtap="showMsg" data-msg="xxx"></view>
    
<!-- 
    js代码
    	showMsg(e){
 			e.target.dataset.msg //获取事件参数
		}
-->
```

表单

```html
	<input value="{{msg}}" bindinput="inputHd"></input>

<!-- 
    js代码
		inputHd(e){
			e.detail.value   //表单的值
		}
-->
```

#### 组件和API

##### 常用组件

- 容器 view
- 内容 text
- 表单 input/button
- 媒体 image


##### 常用方法

- showToast

```javascript
	// 显示轻提示
    wx.showToast({
        title:'成功',
        icon:'success',
        duration:2000
    })
    
    // 关闭轻提示
    wx.hideToast()
```

- showLoading

```javascript
	// 显示loading
    wx.showLoading({
        title:'加载中',
    })
    setTimeout(function(){
    	// 隐藏loading
        wx.hideLoading()
    },2000)
```

- 数据存储

```javascript
	// setStorageSync存储数据
	wx.setStorageSync('key','value')

	// getStorageSync获取数据
	var value = wx.getStorageSync('key')
```

- wx.request请求数据

```javascript
	wx.request({
		url:'https://www.aiav.vip/api/xxxx',	// 请求路径
		success(res) {
			console.log(res.data)		// 响应结果
		}
	})
```

#### 导航

利用navigator标签跳转页面
```html
	 <!-- 基本使用 -->
     <navigator url="/pages/a/a">A页面</navigator>
     
	 <!-- 无历史记录 -->
     <navigator url="/pages/a/a" open-type="redirect">A页面</navigator>
     
     <!-- 切换Tabbar -->
     <navigator url="/pages/a/a" open-type="switchTab">A页面</navigator>
     
     <!-- 返回 -->
     <navigator url="/pages/a/a" open-type="navigateBack">A页面</navigator>
     
     <!-- 传参 -->
     <navigator url="/pages/a/a?id=123">A页面</navigator>
```
js方式跳转

```javascript
		wx.navigateTo({url:''})		// 跳转页面
		wx.redirectTo({url:''})		// 跳转不留历史记录
		wx.switchTab({url:''})		// Tabbar切换
		wx.navigateBack()			// 返回

```

#### 生命周期

- onLoad	        页面初始化，参数为路由跳传 传递的参数
- onShow  		 前台显示
- onReady		页面首次渲染 
- onHide		后台显示
- onUnload		页面卸载(redirect时候，页面就会卸载)
	
#### 页面全局方法

- onPullDownRefresh		  下拉刷新
- onReachBottom 		上拉触底
- onShareAppMessage  	  分享

[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)