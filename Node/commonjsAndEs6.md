<!--
 * @Date: 2021-01-12 16:14:00
 * @LastEditors: Aiva
 * @LastEditTime: 2021-01-12 16:14:13
 * @FilePath: \Gitbook\Node\commonjsAndEs6.md
-->
# CommonJS模块规范和ES6模块规范

#### 什么是模块规范？
- 模块化规范中，每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

###### CommonJS模块规范和ES6模块规范是完全不一样的

#### CommonJS
- CommonJS一般在node.js中使用
- CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。

```
// a.js
var name = "文件a.js";
var sayName= function (name) {
  console.log(name);
};
module.exports.name = name;
module.exports.sayName= sayName;
```
上面代码通过module.exports输出变量name和函数sayName。

- require方法用于加载模块。
```
var example = require('./a.js');
console.log(example.name); // 文件 a.js
example.sayName('张三'); // 张三
```
###### exports 与 module.exports
- 为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。
```
var exports = module.exports;
```
于是我们可以直接在 exports 对象上添加方法，表示对外输出的接口，如同在module.exports上添加一样。注意，不能直接将exports变量指向一个值，因为这样等于切断了exports与module.exports的联系。

 

#### ES6模块规范
- 与CommonJS不同，ES6是用 export 和 import 来导出、导入模块的。

 **导出**
```
// a.js
var name =  'Aiva';
export {name,};
```
 **导入**
```
import { name } from './a'   // 后缀名默认为 js
console.log(name);    // Aiva
```

**需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。**

- export default 命令

**使用export default命令，为模块指定默认输出。**
```
// a.js
export default {
  name:'Aiva'
}
 ```
**导入**
```
import name from './a'   // 后缀名默认为 js
console.log(name);    // Aiva
 ```
**改名**
```
// as 关键字可以更改变量名
import name as n from './a
console.log(name);    // Aiva
```