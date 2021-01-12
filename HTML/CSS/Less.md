# Less

> Less （Leaner Style Sheets 的缩写）是一门 CSS 预处理语言，它扩充了 CSS 语言，增加了诸如变量、混合（mixin）、函数等功能，让 CSS 更易维护、方便制作主题、扩充。Less 可以运行在 Node 或浏览器端。

*less项目越大发挥的作用越大（项目小的时候就显得复杂）* 

### 安装Less
- **Node环境**

    使用npm安装
    ```bash
        // 全局安装
        npm install less -g
    ```
    编译
    ```bash
        // 将 bootstrap.less 编译为 bootstrap.css 文件
        lessc bootstrap.less bootstrap.css
    ```

- **浏览器环境**

    将外部样式表的rel设置为stylesheet/less
    ```html
        <link rel="stylesheet/less" type="text/css" href="paht/styles.less" />
    ```
    引入编译less的js
    ```html
        <script src="paht/less.js" type="text/javascript"></script>
    ```

### 变量

```less
	@width:100px;
	@c:color;

	p {
        width:@width; 
        height:@width*1.5;
        @{color}:red;
	}
```

作为属性时需要用{}括号

### 嵌套

```less
	.header{
     .logo{
          &:hover{
              // ......
          }
      }
     .menu{}
	}
```

### 混合
- 无参混合

    ```less
		.bordered {
            border:1px solid #ddd;
		}
		p{.bordered;}
		h1{.bordered;}
    ```

- 有参数和默认值

	```less
        .setSize(@w:100px,@h:100px){
        	width:@w;
        	height:@h;
        }

        p{.setSize();}
        h1{.setSize(50px,200px);}
	```

- 不定参数

	```less
    .shadow(...){
        box-shadow:@arguments;
    }

    p{shadow(0,2px,0,red)}
	```

- 方法匹配

    ```less 
        .fl(l){float:left;}
        .fl(r){float:right;}

        p{.fl(l)}
        h1{.fl(r)}
    ```

- 嵌套匹配

    ```less 
    .div{
         .header{ 
               border:1px solid red;
               .link{color:red}
          }
    }
    p{.div>.header;}
    h1{.div>.header>.link;}
    ```

### 循环

```less
.genCol(@n,@i:1) when (@i<=@n){
    .col-@{i}0{
        flex-basis: @i*10%;
        width:  @i*10%;
        max-width:  @i*10%;
    }
.genCol(@n,@i+1)
}
.genCol(10);
```

### 避免编译  ~“不需要less编译内容”

```less
	width:~“30px+100px”;
```

### 导入

```less 
	import xxx.less
	// 如果文件后缀名是less，可省略文件后缀
	import less

	@import "xxx.css"
```

### 函数
- 颜色函数

```less
	// lighten(颜色，百分比) 遍亮
	background-color:lighten(#ff0,50%);
	// darken(颜色，百分比) 变暗
	background-color:lighten(#f0f,50%);
```