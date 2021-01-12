# Socket.io

## HTML5 WebSocket
WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

现在，很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

HTML5 定义的 WebSocket 协议，能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。

## Socket.io

> Socket.io是一个WebSocket库，包括了客户端的js和服务器端的nodejs，它的目标是构建可以在不同浏览器和移动设备上使用的实时应用。它会自动根据浏览器从WebSocket、AJAX长轮询、Iframe流等等各种方式中选择最佳的方式来实现网络实时应用，非常方便和人性化，而且支持的浏览器最低达IE5.5。

### Socket.io特点
- 易用性：socket.io封装了服务端和客户端，使用起来非常简单方便。
- 跨平台：socket.io支持跨平台，这就意味着你有了更多的选择，可以在自己喜欢的平台下开发实时应用。
- 自适应：它会自动根据浏览器从WebSocket、AJAX长轮询、Iframe流等等各种方式中选择最佳的方式来实现网络实时应用，非常方便和人性化，而且支持的浏览器最低达IE5.5。


#### 搭建基本结构

服务端
```javascript

// Express 初始化 app 作为 HTTP 服务器的回调函数 
const express = require('express');
const app = express();
const serve = require('http').createServer(app);
const io = require('socket.io')(server);

// 将./public目录配置为服务器的静态目录，使用/static 访问
app.use('/static',express.static('./public'));

// 定义一个路由 / 来处理首页访问
const fs = require('fs');
app.get('/', function(req, res){
	// 读取路径为./views/index.html的文件内容并响应给客户端
    fs.readFile('./views/index.html',(err,data) => {
      if (err) {
          res.send(404,'Not Page!')
      }else {
          res.send(data.toString())
      }
    })
});

// 监听3000端口
http.listen(3000, function(){
  console.log('listening on *:3000');
});

// 监听连接事件
io.on('connection',(socket) => {
    console.log('有一个客户端已连接！')
    
    // 客户端断开连接
	socket.on('disconnect', () => {
		console.log('有一个客户端已断开连接！');
	});
	// 错误
	socket.on('error', () => {
        console.log('发生错误')
    })
})

```
客户端
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Socket.io</title>
</head>
<body>
    <h1>Socket.io</h1>
	<script src="/static/js/socket.io.js"></script>
	<script>
		// 建立连接
		var socket = io.connect('ws://127.0.0.1:3000');
        //监听与服务器端的连接成功事件
        socket.on('connect',function(){
            console.log('连接成功');
        });
        
        //监听与服务器端断开连接事件
        socket.on('disconnect',function(){
           console.log('断开连接');
        });
	</script>
</body>
</html>
```

### 触发事件
Socket.IO 的核心理念就是允许发送、接收任意事件和任意数据。任意能被编码为 JSON 的对象都可以用于传输。二进制数据 也是支持的。

客户端
```html
<script>
	// 建立连接
    var socket = io.connect('ws://127.0.0.1:3000');
    
    // 监听服务端的消息
    socket.on('message',(data) => {
        console.log(data)
    })
    
    // 给服务端发送消息
    socket.emit('chat','hello world!');
    
    // 监听错误
    socket.on('error',() => {
        console.log('发生错误')
    })
    
    // 断开连接
    socket.disconnect();
</script>
```
服务端
```javascript
io.on('connection',(socket) => {
    console.log('有一个客户端已连接！')
    
    // 监听客户端的消息
    socket.on('chat',(data) => {
        console.log(data)
    })
    
    // 给当前连接的这个客户端发送消息
    socket.emit('message','hello world!');
    
    // 给所有已连接的客户端发消息
    io.emit('message','hello world!');
})
```
**emit函数有两个参数**
- 第一个参数是自定义的事件名称,发送方发送什么类型的事件名称,接收方就可以通过对应的事件名称来监听接收
- 第二个参数是要发送的数据

#### 事件总结

服务端事件
| 事件名称 | 解释 |
|----|----|
| connection | 客户端成功连接 |
| message | 接收到客户端的消息 |
| disconnect | 客户端断开连接 |
| error | 监听错误 |

客户端事件
| 事件名称 | 解释 |
|----|----|
| connect | 连接到服务端 |
| message | 接收到服务端的消息 |
| disconnect | 客户端断开连接 |
| error | 监听错误 |

