# H5+ App基础

> HTML5 Plus移动App，简称5+App，是一种基于HTML、JS、CSS编写的运行于手机端的App，这种App可以通过扩展的JS API任意调用手机的原生能力，实现与原生App同样强大的功能和性能。

### 开发环境

[HBuilderX](http://www.dcloud.io/)

### 创建应用

- 启动HBuilderX，在菜单栏中选择“文件”-> “新建”->“项目”
- 选择5+App，在应用名称中输入项目名称，选择项目目录，选择默认模板
- 点击创建

![step1.png](https://upload-images.jianshu.io/upload_images/20584634-6059696adfa86ce8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 调用API


对于HTML5+应用的页面有一个很重要的“plusready”事件，此事件会在页面加载后自动触发，表示所有HTML5+ API可以使用，在此事件触发之前不能调用HTML5+ API，所以应该在此事件回调函数中调用页面初始化需要调用的HTML5+ API，而不应该在onload或DOMContentLoaded事件中调用

```html
<!DOCTYPE html>  
<html>  
<head>  
    <meta charset="utf-8"/>  
    <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no"/>  
    <title>Hello world</title>  
    <script type="text/javascript">   
// 扩展API是否准备好，如果没有则监听“plusready"事件  
if(window.plus){  
    plusReady();  
}else{   
    document.addEventListener( "plusready", plusReady, false );  
}  
// 扩展API准备完成后要执行的操作  
function plusReady(){    
	// 输出设备的生产厂商
    document.querySelector('#vendor').innerText = plus.device.vendor
}  
    </script>   
</head>   
<body>
	<p>设备厂商：<span id="vendor"></span></p>
</body>  
</html>
```

#### device

Device模块管理设备信息，用于获取手机设备的相关信息，如IMEI、IMSI、型号、厂商等。通过plus.device获取设备信息管理对象。

**常用属性**

- imei: 设备的国际移动设备身份码
- imsi: 设备的国际移动用户识别码
- model: 设备的型号
- vendor: 设备的生产厂商
- uuid: 设备的唯一标识
- networkinfo: 用于获取网络信息

*networkinfo是一个对象*

```javascript
	//设备的国际移动设备身份码
	plus.device.imei
	
	//设备的网络信息
	plus.networkinfo
```

#### runtime

Runtime模块管理运行环境，可用于获取当前运行环境信息、与其它程序进行通讯等。通过plus.runtime可获取运行环境管理对象。


**常用方法**

- restart: 重启
- quit：退出

```javascript
	// 重启APP
	plus.runtime.restart()
	
	// 退出APP
	plus.runtime.quit()
```

#### camera

Camera模块管理设备的摄像头，可用于拍照、摄像操作，通过plus.camera获取摄像头管理对象。

拍照

```javascript
// getCamera返回获取摄像头管理对象，调用该对象的captureImage方法进行拍照操作，该方法接受3个参数，1.成功的回调函数，有一个路径参数，必选 2.失败的回调函数  3. 摄像头拍照参数
plus.camera.getCamera().captureImage((path) => {
    // ...code
})
```


#### gallery

Gallery模块管理系统相册，支持从相册中选择图片或视频文件、保存图片或视频文件到相册等功能。通过plus.gallery获取相册管理对象。

**常用方法**

- pick: 从系统相册选择文件（图片或视频）
- save: 保存文件到系统相册中

```javascript
// 从相册中选择图片 
function galleryImg() {
	// 从相册中选择图片
	console.log("从相册中选择图片:");
    plus.gallery.pick( function(path){
    	console.log(path);
    }, function ( e ) {
    	console.log( "取消选择图片" );
    }, {filter:"image"} );
}

// 保存图片到相册中 
function savePicture() {
	plus.gallery.save( "_doc/img.jpg", function () {
		alert( "保存图片到相册成功" );
	} );
}
	
```


#### nativeUI

nativeUI管理系统原生界面，可用于弹出系统原生提示对话框窗口、时间日期选择对话框、等待对话框等。

**常用方法**

- alert: 弹出系统提示对话框
- confirm: 弹出系统确认对话框
- previewImage: 预览图片
- showWaiting: 显示系统等待对话框
- pickDate: 弹出系统日期选择对话框
- pickTime: 弹出系统时间选择对话框
- prompt: 弹出系统输入对话框
- toast: 显示自动消失的提示消息


```html
<script>
    function wait() {
        let w = plus.nativeUI.showWaiting('正在等待系统响应');
        setTimeout(() => {
            w.close()
        }, 2000)
    }
</script>

<button onclick="wait()">系统等待</button>

<button onclick="plus.nativeUI.toast('欢迎')">Toast轻提示</button>

<button onclick="plus.nativeUI.alert('警告弹窗？',function () {},'Alert警告','关闭警告')">Alert警告</button>

<button onclick="plus.nativeUI.confirm('今天天气不错！')">confirm对话框</button>

<button onclick="plus.nativeUI.prompt('请输入内容')">prompt输入框</button>

<button onclick="plus.nativeUI.previewImage(['https://i.loli.net/2020/02/20/koUiZ8OCDabPhYt.jpg'])">预览图片</button>

<button onclick="plus.nativeUI.pickDate((e) => {plus.nativeUI.toast(e.date)})">选择日期</button>

<button onclick="plus.nativeUI.pickTime((e) => {plus.nativeUI.toast(e.date)})">选择时间</button>
```


#### webview

Webview模块管理应用窗口界面，实现多窗口的逻辑控制管理操作。通过plus.webview可获取应用界面管理对象。

**常用方法**

- close: 关闭Webview窗口
- create: 创建新的Webview窗口
- currentWebview: 获取当前窗口的WebviewObject对象

```javascript
	// 获取当前Webview窗口对象
	var ws=plus.webview.currentWebview();
	console.log( "当前Webview窗口："+ws.getURL() );
	
	// 创建并显示新窗口
	var w = plus.webview.create('https://aiav.vip');
	w.show(); // 显示窗口
	
	// 关闭Webview窗口

	var ws=plus.webview.currentWebview();
	plus.webview.close(ws);
```



完整API详情请查看[官方API文档](http://www.html5plus.org/doc/zh_cn/accelerometer.html)