# Angular2.x 组件基础

### 创建组件

在项目根目录运行 	ng g c 文件夹/组件名称
```bash
	ng g c components/child
```

创建完成后会生成组件文件夹

- .html 	模板文件
	 .css 		CSS
	 .ts 		组件类
	 .spc.ts 	  测试文件


#### 组件的装饰器

```javascript
@Component({
  selector: 'app-child',	// 引用的标签
  templateUrl: './child.component.html',	// 组件的模板路径
  styleUrls: ['./child.component.css']		// 组件的css路径
})
```

#### 组件的类

```javascript
export class ChildComponent implements OnInit {

}

// implements 实现接口
// OnInit 组件的生命周期-初始化
```

### 组件传参

- 父组件给子组件传参

```html
	<app-child [list]='list'></app-child>
```

```javascript
import { Component, OnInit,Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {
  // 使用@Input接收
  @Input() list:string
  current:any
  
  constructor() { 
    this.current = null
   }

  ngOnInit(): void {
    
  }

}

```

- 子组件给父组件传参

```javascript
import { Component, OnInit,Input,Output,EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {
  @Input() list:string
  current:any
  // 使用Output和EventEmitter模块发送参数
  @Output() send = new EventEmitter<number>()
  constructor() { 
    this.current = null
   }

  ngOnInit(): void {
    
  }

  setCurrent(i) {
    this.current = i;
    this.send.emit(i)
  }

}

```

```html
<!-- 通过getType方法接受子组件传过来的参数 -->
<app-child [list]='list' (send)="getType($event)"></app-child>
```