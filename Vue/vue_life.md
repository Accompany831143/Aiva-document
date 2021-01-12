# Vue的生命周期

#### 什么是生命周期
从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期
- 生命周期钩子是生命周期事件的另一种称呼
- 生命周期钩子，生命周期函数都是指生命周期事件

先上图
![image](https://upload-images.jianshu.io/upload_images/20584634-a35a6f1cb200e502.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注：

**创建期间**

- beforeCreate：vue实例被创建之前执行，此时，data 和 methods 属性还未被初始化

- created：vue实例已经创建完成，此时 data 和 methods 已经创建完毕可以访问，但此时还没有开始编译模板

- beforeMount：完成模板编译，但是还没有挂载到页面中

- mounted：将编译好的模板挂载到页面指定的容器中

**运行期间**

- beforeUpdate：data中的值已经改变，但是DOM节点未渲染，页面上还是没有更新的数据

- updated：重新渲染Dom节点，data中的数据和页面中显示的数据一致

**销毁期间**

- beforeDestroy：vue实例销毁之前调用。但这时实例仍然完全可用。

- destroyed：vue 实例销毁后调用。Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 