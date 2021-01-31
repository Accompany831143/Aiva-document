# JavaScript预编译（执行期上下文）

### 什么是预编译？
前端同学都知道js里有个叫变量声明提升的概念，其实这就是预编译造成的。预编译也叫执行期上下文。当js代码运行的时候，实际上是运行在执行期上下文里面，执行期上下文包含三种。
- 全局上下文（Global Object，即全局代码,简称GO）
- 函数上下文 （Activation Object，每个函数在运行的前一刻会创建属于自己的执行期上下文，简称AO）
- eval方法也会创建新的执行期上下文 (*用的不多，这里就不说它了*)

```javascript
  var a = 1;
  var b = 2;
  
  function add(x,y) {
	  return x + y;
  }
  
  add(1,2)
  
  // 1. 执行代码，首先创建一个全局的GO
  // 2. 运行add函数的前一刻创建属于add函数的AO
```

### 执行期上下文栈
1. 全局代码执行前,JS就会创建一个栈来储存管理所有的执行上下文对象
2. 全局执行期上下文确定后，把它添加到栈中（栈底）
3. 当函数的执行期上下文创建完后，把它添加到栈中（栈顶）
4. 当前函数执行完毕，在栈顶把这个函数的执行期上下文移出
	- **函数执行中,遇到return直接终止可执行的代码,会直接将当前上下文弹出栈**
5. 全部代码执行完毕，栈里就只剩全局的执行期上下文了
6. 当浏览器窗口关闭，移出全局的执行期上下文

### 执行期上下文创建过程
#### AO
1. 创建AO对象(Activation Object)
2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined
3. 将实参和形参相统一
4. 在函数体里面找函数声明，值赋予函数体

```javascript
  function fn(a,b){
    console.log(a);
    c = 0;
    var c;
    a = 3;
    b = 2;
    console.log(b);
    function b(){};
    function d(){};
    console.log(b);
  }
  fn(1,2)
  /*
	创建过程：
		1. 创建AO对象
		AO = {
			
		}
		2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined
		AO = {
			a:undedfined,
			b:undefined,
			c:undefined
		}
		3.将实参和形参相统一
		AO = {
			a:1,
			b:2,
			c:undefined
		}
		4. 在函数体里面找函数声明，值赋予函数体
		AO = {
			a:1,
			b:function b(){},
			c:undefined,
			d:function b(){}
		}
  */
  /*
    函数执行过程：
	function fn(a,b){
	  console.log(a); // 1
	  c = 0;          // AO的c变成0
	  var c;		  // AO创建过了，忽略
	  a = 3;          // AO的a变成3
	  b = 2;          // AO的b变成2
	  console.log(b); // 2
	  function b(){}; // AO创建过了，忽略
	  function d(){}; // AO创建过了，忽略
	  console.log(b); // 2
	}
  */
	
	
```

#### GO
1. 创建GO对象（Global Object）
2. 找形参和变量声明，将变量和形参名作为 GO 属性名，值为 undefined
3. 在函数体里面找函数声明，值赋予函数体

```javascript
  console.log(a)
  var a= 1;
  var b;
  function c() {};
  function b() {};
  console.log(c)
  console.log(b)
  b = 2;
  console.log(a)
  console.log(b)
  /*
	创建过程：
		1. 创建AO对象
		AO = {
			
		}
		2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined
		AO = {
			a:undedfined,
			b:undefined,
			c:undefined
		}
		3. 在函数体里面找函数声明，值赋予函数体
		AO = {
			a:undedfined,
			b:function b(){},
			c:function c(){},
		}
  */
  /*
    函数执行过程：
	console.log(a) // undefined
	var a= 1;      // GO的a变成1
	var b;   	   // GO创建过了，忽略
	function c();  // GO创建过了，忽略
	function b();  // GO创建过了，忽略
	console.log(c) // function c() {}
	console.log(b) // function b() {}
	b = 2;         // GO的b变成2
	console.log(a) // 1
	console.log(b) // 2
  */
	
	
```

**其实全局对比函数的执行期上下文创建过程就是少了个形参和实参统一，其余的都一样**