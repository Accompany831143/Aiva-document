# ReactNative的基本使用

React Native 是一款使用JavaScript和React编写原生移动应用的框架，熟练使用React的开发者可以很容易的使用React Native来开发APP，本文介绍一下React Native的安装和基本使用。

[React Native 中文网](https://reactnative.cn/)

## 安装

本文是在window环境下安装的，其他环境请参考官方文档。

#### 1. 安装必需软件

- **Node**  (版本需大于等于 12)
- **Python2**  (版本必须为 2.x（不支持 3.x）)
- **JDK**  (版本必须是 1.8)


#### 2. 安装nrm,yarn

```bash
	npm install nrm -g		// 安装nrm
	npx nrm use taobao		// 切换淘宝源
	npm install yarn -g		// 安装yarn
```
#### 3. 安装Android Studio

1. 下载安装Android Studio
2. 安装过程中Custom选项先不勾选
3. 安装完成后在 Android Studio 的欢迎界面中点击"Configure"，然后点击SDK Manager
4. 在 SDK Manager 中选择"SDK Platforms"选项卡，然后在右下角勾选"Show Package Details"。展开Android 9 (Pie)选项，确保勾选了下面这些组件：

![image](http://upload-images.jianshu.io/upload_images/20584634-bfd72b08fa6c2b17?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 然后点击"SDK Tools"选项卡，同样勾中右下角的"Show Package Details"。展开"Android SDK Build-Tools"选项，确保选中了 React Native 所必须的28.0.3版本。你可以同时安装多个其他版本。

6. 配置 ANDROID_HOME 环境变量

React Native 需要通过环境变量来了解你的 Android SDK 装在什么路径，从而正常进行编译。
打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量 -> 新建，创建一个名为ANDROID_HOME的环境变量（系统或用户变量均可），指向你的 Android SDK 所在的目录（具体的路径可能和下图不一致，请自行确认）

![image](http://upload-images.jianshu.io/upload_images/20584634-685a201b0a14e22e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


SDK 默认是安装在下面的目录：
```
c:\Users\你的用户名\AppData\Local\Android\Sdk
```

你可以在 Android Studio 的"Preferences"菜单中查看 SDK 的真实路径，具体是Appearance & Behavior → System Settings → Android SDK。

7. 把一些工具目录添加到环境变量 Path 中

打开控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 高级 -> 环境变量，选中Path变量，然后点击编辑。点击新建然后把这些工具目录路径添加进去：platform-tools、emulator、tools、tools/bin

```bash
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

#### 4. 创建项目

找到你需要创建项目的目录运行

```bash
	npx react-native init myApp
```

**Windows 用户请注意，请不要在某些权限敏感的目录例如 System32 目录中 init 项目！会有各种权限限制导致不能运行！**

#### 5. 下载木木模拟器

#### 6. 由于第一次运行时需要下载大量编译依赖，这里建议使用阿里云的maven镜像

修改 android/build.gradle

```gradle
allprojects {
    repositories {
        mavenLocal()
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' } 
        ....
}
```

#### 7. 连接模拟器

木木模拟器默认端口为 7555

项目根目录运行

```bash
	adb connect 127.0.0.1:7555
```
连接成功会有提示

#### 8. 运行项目

在你的项目目录中运行**yarn android** 或者**yarn react-native run-android**

*第一次运行耗时较长*


## 基本使用



#### 入门Hello World

编辑App.js

```javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

export default class HelloWorldApp extends Component {
  render() {
    return (
        <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
          <Text>Hello, world!</Text>
        </View>
    );
  }
}
```

React Native 看起来很像 React，只不过其基础组件是原生组件而非 web 组件。

#### 组件

一个 App 的最终界面，其实也就是各式各样的组件的组合。组件本身结构可以非常简单——唯一必须的就是在render方法中返回一些用于渲染结构的 JSX 语句。

- 核心组件

| 原生组件      | 安卓          | IOS             | HTML         | 描述                                                        |
| ------------- | ------------- | --------------- | ------------ | ----------------------------------------------------------- |
| <View\>       | <ViewGroup\>  | <UIView\>       | 非滚动<div\> | 一个支持Flexbox，样式，一些触摸处理和辅助功能控件布局的容器 |
| <Text\>       | <TextView\>   | <UITextView\>   | <p\>         | 显示，设置样式和嵌套文本字符串，甚至处理触摸事件            |
| <Image\>      | <ImageView\>  | <UIImageView\>  | <img\>       | 显示不同类型的图像                                          |
| <ScrollView\> | <ScrollView\> | <UIScrollView\> | <div\>       | 通用滚动容器，可以包含多个组件和视图                        |
| <TextInput\>  | <EditText\>   | <UITextField\>  | <input\>     | 允许用户输入文本                                            |




- 文本输入框组件

TextInput是一个允许用户输入文本的基础组件。它有一个名为onChangeText的属性，此属性接受一个函数，而此函数会在文本变化时被调用。另外还有一个名为onSubmitEditing的属性，会在文本被提交后（用户按下软键盘上的提交键）调用。


- 触摸组件（Button）

```javascript
<Button onPress={()=>alert("你好世界")} title="确定" color="#f30"></Button>
```

其中onPress是回调函数，title是按钮上显示的文字，color是按钮颜色；

也可以作为警告框，拥有多个选项，从而弹出不同内容：

```javascript
<Button 
    onPress={()=>Alert.alert("小胖别吃了好吗？","你已经160斤了！",[
        {text:'不行',onPress:()=>alert("小胖：不行我饿")},
        {text:'好吧',onPress:()=>alert("小胖：好吧我不吃了")},
        {text:'难过',onPress:()=>alert("小胖：我是单身狗，胖点怕啥")},
    ])}
    title="询问小胖"
    color="blue">
</Button>
```
可以看到我们设置了三个选项，每一项都会弹出不同的内容，内容也是自己设置的；


- 触摸组件（Touchable 系列组件）

这个组件的样式是固定的。所以如果它的外观并不怎么搭配你的设计，那就需要使用TouchableOpacity或是TouchableNativeFeedback组件来定制自己所需要的按钮

1. 使用TouchableHighlight来制作按钮或者链接。注意此组件的背景会在用户手指按下时变暗。

2. 在 Android 上还可以使用TouchableNativeFeedback，它会在用户手指按下时形成类似墨水涟漪的视觉效果。

3. TouchableOpacity会在用户手指按下时降低按钮的透明度，而不会改变背景的颜色。

4. 如果想在处理点击事件的同时不显示任何视觉反馈，则需要使用TouchableWithoutFeedback

5. 某些场景中可能需要检测用户是否进行了长按操作。可以在上面列出的任意组件中使用onLongPress属性来实现。


- 滚动视图

ScrollView是一个通用的可滚动的容器，可以在其中放入多个组件和视图，而且这些组件并不需要是同类型的。ScrollView 不仅可以垂直滚动，还能水平滚动（通过horizontal属性来设置）。
ScrollView适合于显示数量不多的滚动元素。放置在ScrollView中的所有组件都会被渲染


- 长列表

FlatList组件用于显示一个垂直的滚动列表，其中的元素之间的结构近似而仅数据不同。
FlatList和ScrollView不同的是，FlatList并不立即渲染所有元素，而是优先渲染屏幕上可见的元素。
FlatList组件必须的两个属性是data和renderItem。data是列表的数据源，而renderItem则从数据源中逐个解析数据，然后返回一个设置好格式的组件来渲染。
例如渲染数据：

```javascript
<FlatList
    keyExtractor={item=>item.ID}
    data={this.state.movies}
    numColumns={3}   //一行显示3个
    columnWrapperStyle={styles.row} //增加下方写的样式
    renderItem={({item})=>{return(
        <View style={{marginTop:20}} key={item.ID}>
        <Image 
          style={{width:135,height:160}} 
          source={{uri:item.defaultImage}}>
        </Image>
        <Text style={{marginTop:5,textAlign:"center"}}>{item.MovieName}</Text>
        </View>
    )}}
/>

/*
flatList长列表组件，
keyExtractor是key抽取
data数据指定
numColumns行的数量，
renderItem列的渲染
columnWrapperStyle行的样式指定
*/
```


*更多组件请参考官方文档*

#### Props

```javascript
render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}} />
    );
}
```

{pic}外围有一层括号，我们需要用括号来把pic这个变量嵌入到 JSX 语句中。括号的意思是括号内部为一个 js 变量或表达式，需要执行后取值。

自定义的组件也可以使用props。通过在不同的场景使用不同的属性定制，可以尽量提高自定义组件的复用范畴。只需在render函数中引用this.props，然后按需处理即可。下面是一个例子：


```javascript
import React, { Component } from 'react';
import { Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <View style={{alignItems: 'center', marginTop: 50}}>
        <Text>Hello {this.props.name}!</Text>
      </View>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}
```

最终3个名字会被依次输出。

#### State

React Native使用两种数据来控制一个组件：props和state。props是在父组件中指定，而且一经指定，在被指定的组件的生命周期中则不再改变。对于需要改变的数据，我们需要使用state。

一般来说，需要在class中声明一个state对象，然后在需要修改时调用setState方法。典型的场景是在接收到服务器返回的新数据，或者在用户输入数据之后。你也可以使用一些“状态容器”比如Redux来统一管理数据流。
每次调用setState时，BlinkApp 都会重新执行 render 方法重新渲染。

**注意**

- 一切界面变化都是状态state变化
- state的修改必须通过setState()方法
- this.state.likes = 100; // 这样的直接赋值修改无效！
- setState 是一个 merge 合并操作，只修改指定属性，不影响其他属性
- setState 是异步操作，修改不会马上生效


#### 样式的书写

行内样式

```javascript
 <View>
           <Text style={[styles.red,styles.big]}>我是内联样式文本</Text>
           <Text style={{fontWeight:"bold"}}>我是行内样式文本</Text>
           <Text style={{...styles.big,color:"yellow", fontStyle:"italic"
}}>我是内联+行内混合样式文本</Text>
</View>
```

在render函数外部定义样式


```javascript
const styles = StyleSheet.create({
  center:{justifyContent:"center",alignItems:"center"},
  row:{flexDirection:"row",justifyContent:"space-around"},
  col:{flex:1,height:180},
  big:{fontSize:48},
  red:{color:"#f30"}
});
```

#### 网络请求

一般长列表的数据都是通过网络请求获取的；然后渲染到页面；
传统方式如下：


```javascript
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',   //请求方式
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',    //请求头
  },
  body: 'key1=value1&key2=value2',     //请求参数
});
```

上面的示例演示了如何发起请求。很多情况下，还需要处理服务器回复的数据。网络请求天然是一种异步操作；异步的意思是应该提取方法会返回一个Promise，这种模式可以简化异步风格的代码;
如下所示：

```javascript
function getMoviesFromApiAsync() {   //获取电影xx的方法，自己创建的
  return fetch('https://facebook.github.io/react-native/movies.json')   //接口网站
    .then((response) => response.json())    //将请求到的数据转为json格式
    .then((responseJson) => {
      return responseJson.movies;   //请求成功返回数据
    })
    .catch((error) => {
      console.error(error);    //请求失败，输出错误
    });
}
```