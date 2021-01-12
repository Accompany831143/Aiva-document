# ReactNative 导航的基本使用

> 移动应用基本不会只由一个页面组成。管理多个页面的呈现、跳转的组件就是我们通常所说的导航器（navigator）。React Navigation 提供了简单易用的跨平台导航方案，在 iOS 和 Android 上都可以进行翻页式、tab 选项卡式和抽屉式的导航布局。

[官方文档](https://reactnavigation.org/docs/getting-started)


### 安装
```bash
npm install @react-navigation/native	原生导航
npm install @react-navigation/stack		堆栈导航
npm install @react-navigation/bottom-tabs	底部导航
npm install @react-navigation/material-top-tabs react-native-tab-view 	顶部安卓导航

npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view	依赖
```

### 配置
将以下两行添加到中的dependencies部分android/app/build.gradle

```
implementation 'androidx.appcompat:appcompat:1.1.0-rc01'
implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0-alpha02'
```

 

## 堆栈导航

- NavigationContainer 路由容器
- createStackNavigator 创建堆栈导航方法


**1. 导入组件**

```
// 导入导航容器
import { NavigationContainer } from '@react-navigation/native';
// 导入创建堆栈导航方法
import { createStackNavigator } from '@react-navigation/stack';
```

**2. 创建导航**

```
const Stack = createStackNavigator();
```

**3. 创建导航视图**
```
function Home() {
  return (
    <View>
      <Text>首页</Text>
    </View>
  );
}

function Details() {
  return (
    <View>
      <Text>详情页面</Text>
    </View>
  );
}

```
**4. 包装页面**
```
function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={Home} />
        <Stack.Screen name="Details" component={Details} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;


```

### 导航跳转

- navigation.push('Details')        切换页面
- navigation.replace('Details')     替换页面
- navigation.goBack()               返回
- navigation.popToTop()             回到最顶层页

```javascript
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')} />
        <Button
        title="再一次跳转详情"
        onPress={() => navigation.push('Details')} />
        <Button 
            title="返回" 
            onPress={() => navigation.goBack()} />
        <Button
            title="回到顶层"
            onPress={() => navigation.popToTop()} />
    </View>
  );
}
```


###  导航传参

传递参数

```javascript
<Button
    title="跳转到详情"
    onPress={() => {
        navigation.navigate('Details', {
            itemId: 86,
            otherParam: '你想传递参数',
        });
    }}
/>
```

接收参数   

通过props的route.params中获取参数

```
function DetailsScreen({ route, navigation }) {
  const { itemId } = route.params;
  const { otherParam } = route.params;
  return (...)
}
```

初始化参数

```
 <Stack.Screen
  name="Details"
  component={DetailsScreen}
  initialParams={{ itemId: 42 }}
/>
```

###  配置标题


##### 设置标题
 ```
 <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{ title: '首页' }}
/>
 ```


##### 更新options
```
<Button
  title="更新标题"
  onPress={() => navigation.setOptions({ title: '新标题' })}
/>
```

##### 标题样式
```
<Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{
      title: 'My home',
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    }}
      />
```
##### options跨屏幕共享
```
<Stack.Navigator
  screenOptions={{
    headerStyle: {
      backgroundColor: '#f4511e',
    },
    headerTintColor: '#fff',
    headerTitleStyle: {
      fontWeight: 'bold',
    },
  }}
>
  <Stack.Screen
    name="Home"
    component={HomeScreen}
    options={{ title: 'My home' }}
  />
</Stack.Navigator>
```

### 用自定义组件替换标题
```

function StackScreen() {
  return (
    <Stack.Navigator>
      <Stack.Screen
        name="Home"
        component={HomeScreen}
        options={{ headerTitle: props => <image source={}>}}
      />
    </Stack.Navigator>
  );
}
```

### 标题按钮

```
<Stack.Screen
        name="Home"
        component={Home}
        options={{
          headerRight: () => (
            <Button
              onPress={() => alert('设置被点击')}
              title="设置"
              color="#fff"
            />
          ),
        }}
/>
```
### StackNavigator navigationOptions 导航页面的属性和方法
```
title       - 可用作的headerBackTitle的标题。

header      - 函数返回一个React元素，用来作为标题。设置null会隐藏标题。
headerRight - 接受React Element将会显示在标题的右侧
headerStyle - 顶部栏的样式
headerTitleStyle - 标题组件的样式

```


### 扩展

navigationOptions 标签栏的属性和方法
```
title - 通用标题可以用作headerTitle和tabBarLabel。

tabBarVisible - 显示或隐藏底部标签栏，默认为true，不隐藏。

tabBarIcon - React Element或给定{focused：boolean，tintColor：string}的函数返回一个React.Node，用来显示在标签栏中。

tabBarLabel - 接收字符串、React Element或者给定{focused：boolean，tintColor：string}的函数返回一个React.Node，用来显示在标签栏中。如果未定义，会使用title作为默认值。如果想要隐藏，可以参考上面的tabBarOptions.showLabel。

tabBarOnPress - 标签栏点击事件回调，接受一个对象，其中包含如下：
```
![image](http://upload-images.jianshu.io/upload_images/1338142-d159d429fe9f22dc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

```
tabBarOnPress: async (obj: any) => {
console.log(obj);
    try {
        const userData = await AsyncStorage.getItem('USER_INFO');
        if (userData) {
            obj.defaultHandler();
        }
        else {
            obj.navigation.navigate('Login');
        }
    } catch (e) {
        Toast.show(e.message, 'center', 1000);
    }
}
```