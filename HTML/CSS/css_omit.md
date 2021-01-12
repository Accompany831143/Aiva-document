# 实现单行、多行文本溢出显示省略号

> 在项目中经常会用到文本溢出显示省略号的需求，这篇文章记录一下如何实现。

### 单行溢出显示省略号
- 设置overflow:hidden 
- 设置宽度（兼容部分浏览器）
- 设置显示文本不换行
- 设置text-overflow:ellipsis

```css

.box {
    overflow: hidden;
    width: 300px;
    white-space: nowrap;
    text-overflow:ellipsis;
}
```
### 多行溢出显示省略号

先上代码

```css
    /*
    -webkit-line-clamp用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的WebKit属性。常见结合属性：
    display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
    -webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
    */
    .box {
        width: 350px;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 3;
        overflow: hidden;
        background-color:#ddd;
    }
```
**因使用了WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端**
