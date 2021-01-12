# 百度地图的简单使用


> 百度地图JavaScript API GL v1.0是一套由JavaScript语言编写的应用程序接口，可帮助您在网站中构建功能丰富、交互性强的地图应用，支持PC端和移动端基于浏览器的地图应用开发，且支持HTML5特性的地图开发。

[官方文档](http://lbsyun.baidu.com/index.php?title=jspopular3.0/guide/show)

###  准备

使用百度地图首先要在百度地图开发平台注册一个开发者账号，然后创建应用，之后你会有一个开发者ak，使用ak就可以访问API接口了。

[百度地图开放平台官网](http://lbsyun.baidu.com/)

### 基本使用

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>百度地图 - 入门</title>
		<style>
			body {
				margin:0;
			}
			#box {
				height:500px;
			}
		</style>
		
		<!-- 引入链接 -->
		<script type="text/javascript" src="http://api.map.baidu.com/api?v=3.0&ak=你的ak"></script>
	</head>
	<body>
		<div id="box"></div>
		
		<script type="text/javascript">
			// 创建地图 box为你要挂载的dom元素ID
			var map = new BMap.Map("box");
			// 创建点坐标
			var point = new BMap.Point(116.404, 39.915);
			// 初始化地图，设置中心点坐标和地图级别
			map.centerAndZoom(point,15)
			
			map.enableScrollWheelZoom(true);     //开启鼠标滚轮缩放
			

		</script>
	</body>
</html>

```

### 添加控件

```javascript


map.addControl(new BMap.NavigationControl());   // 地图导航
map.addControl(new BMap.ScaleControl());        // 缩放控件
map.addControl(new BMap.OverviewMapControl());  //概况
map.addControl(new BMap.MapTypeControl());      //地图类型
```


### 添加标注

```javascript
// 默认标注样式

var point = new BMap.Point(116.404, 39.915);   
var marker = new BMap.Marker(point);        // 创建标注   
map.addOverlay(marker);                     // 将标注添加到地图中


// 自定义图标的标注

let Icon = new BMap.Icon(
	// 图片的地址
    './xxx.png',
    // 显示图片的大小
    new BMap.Size(36,42),
    // 把原始图片缩小为 36,42， 偏移到底部中心区域，（默认是左上角）
    {imageSize:new BMap.Size(36,42),anchor:new BMap.Size(18,42)})


var marker = new BMap.Marker(point,{icon:Icon}) 
// 添加标注
map.addOverlay(marker);

// 监听标注事件
marker.addEventListener("click", function(){   
    alert("您点击了标注");   
});
```

### 添加折线

在地图上绘制为一系列直线段。可以自定义这些线段的颜色、粗细和透明度。颜色可以是十六进制数字形式（比如：#ff0000）或者是颜色关键字（比如：red）

```javascript
var polyline = new BMap.Polyline([
    new BMap.Point(116.399, 39.910),
    new BMap.Point(116.405, 39.920)
    ],
    {strokeColor:"blue", strokeWeight:6, strokeOpacity:0.5}
    );
map.addOverlay(polyline);
```



### 信息窗口

```javascript

// 基本使用
var opts = {    
    width : 250,     // 信息窗口宽度    
    height: 100,     // 信息窗口高度    
    title : "Hello"  // 信息窗口标题   
}    
var infoWindow = new BMap.InfoWindow("World", opts);  // 创建信息窗口对象    
map.openInfoWindow(infoWindow, map.getCenter());      // 打开信息窗口


// 显示html内容

// 内容
let str = `<div class="info"><p>一入前端深似海，重此xxxx是路人.</p></div>`;

var info  = new BMap.InfoWindow(
   str, 
   {title:'web前端大本营'})

marker.addEventListener("click",()=>{
    map.openInfoWindow(info,point);
    // 当marker被单击时候打开信息窗口 
})
```

### 城市检索
search方法提供根据关键字检索特定POI信息服务。

```javascript
var map = new BMap.Map("box");      
map.centerAndZoom(new BMap.Point(116.404, 39.915), 11);      
var local = new BMap.LocalSearch(map, {      
    renderOptions:{map: map}      
});      
local.search("景点");
```

### 路径规划

百度地图JavaScript API v3.0提供了驾车、公交、步行和骑行四种出行方式的路线规划功能。开发者在使用线路规划的接口时，可以使用我们提供的默认展示效果。也可以通过监听事件回调，使用检索数据完成自己的需求

1. 基础驾车路线规划服务

```javascript
var map = new BMap.Map("box"); 
map.centerAndZoom(new BMap.Point(116.404, 39.915), 14); 
var driving = new BMap.DrivingRoute(map, { 
    renderOptions: { 
        map: map, 
        autoViewport: true 
} 
});
var start = new BMap.Point(116.310791, 40.003419);
var end = new BMap.Point(116.486419, 39.877282);
driving.search(start, end);
```

2. 公交路线规划

```javascript
var map = new BMap.Map("box"); 
map.centerAndZoom(new BMap.Point(116.404, 39.915), 14); 
var transit = new BMap.TransitRoute(map, { 
    renderOptions: { 
        map: map, 
        autoViewport: true 
    } 
});
var start = new BMap.Point(116.310791, 40.003419);
var end = new BMap.Point(116.486419, 39.877282);
transit.search(start, end);
```

3. 步行路线规划

```javascript
var map = new BMap.Map("box"); 
map.centerAndZoom(new BMap.Point(116.404, 39.915), 14); 
    var walk = new BMap.WalkingRoute(map, { 
    renderOptions: {map: map} 
}); 
var start = new BMap.Point(116.310791, 40.003419);
var end = new BMap.Point(116.486419, 39.877282);
walk.search(start, end);
```

### 逆/地址解析
地址解析服务提供从地址转换到经纬度的服务，反之，逆地址解析则提供从经纬度坐标转换到地址的转换功能

- 地址解析服务

	根据地址描述获得坐标信息
	百度地图API提供Geocoder类进行地址解析，您可以通过Geocoder.getPoint()方法来将一段地址描述转换为一个坐标。

	如下示例，我们将地址“北京市海淀区上地10街10号”转换获取该位置的地理经纬度坐标，并在这个位置上添加一个标注。注意：在调用Geocoder.getPoint()方法时您需要提供地址解析所在的城市（本例为“北京市”）。
	
```javascript
var map = new BMap.Map("l-map");      
map.centerAndZoom(new BMap.Point(116.404, 39.915), 11);      
// 创建地址解析器实例     
var myGeo = new BMap.Geocoder();      
// 将地址解析结果显示在地图上，并调整地图视野    
myGeo.getPoint("北京市海淀区上地10街10号", function(point){      
    if (point) {      
        map.centerAndZoom(point, 16);      
        map.addOverlay(new BMap.Marker(point));      
    }      
 }, 
"北京市");
```

- 逆地址解析服务

1. 根据坐标点获得该地点的地址描述，是地址解析的逆向转换。

您可以通过Geocoder.getLocation()方法获得地址描述。当解析工作完成后，您提供的回调函数将会被触发。如果解析成功，则回调函数的参数为GeocoderResult对象，否则为null。
在构造Geocoder对象时，可以增加参数{extensions_town: true}来获得乡镇级数据，仅限国内。

```javascript
// 指定经纬度获取地址
var map = new BMap.Map("l-map");      
map.centerAndZoom(new BMap.Point(116.404, 39.915), 11);      
// 创建地理编码实例, 并配置参数获取乡镇级数据
var myGeo = new BMap.Geocoder({extensions_town: true});      
// 根据坐标得到地址描述    
myGeo.getLocation(new BMap.Point(116.364, 39.993), function(result){      
    if (result){      
    alert(result.address);      
    }      
});


// 鼠标点击地图获取地址
var map = new BMap.Map("allmap");
var point = new BMap.Point(116.331398,39.897445);
map.centerAndZoom(point,12);
var geoc = new BMap.Geocoder();    
map.addEventListener("click", function(e){        
    var pt = e.point;
    geoc.getLocation(pt, function(rs){
        var addComp = rs.addressComponents;
        alert(addComp.province + ", " + addComp.city + ", " + addComp.district + ", " + addComp.street + ", " + addComp.streetNumber);
    });        
});

```
