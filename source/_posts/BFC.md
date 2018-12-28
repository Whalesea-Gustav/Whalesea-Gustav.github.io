---
title: BFC
date: 2018-09-17 19:47:39
tags: [CSS, Font-end]
categories: 网络编程
---
# BFC

---

浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的块级格式化上下文

在一个块级格式化上下文里，盒子从包含块的顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。两个相邻的块级盒子的垂直外边距会发生叠加。

在块级格式化上下文中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘），即使存在浮动也是如此，除非这个盒子创建一个新的块级格式化上下文



**BFC是一个CSS2.1中的一个概念，是页面中的一块渲染区域，并且包含一套渲染规则，其决定了子元素将如何定位，以及和其他元素的关系，and相互作用**

## BFC规则：

---

1. **内部**的`Box`会在垂直方向，从顶部开始一个接一个地放置
2. `Box`垂直方向的距离由`margin`决定。属于同一个BFC的两个相邻`Box`的`margin`会发生叠加
3. 每个元素的`margin box`的左边， 与包含块`border box`的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
4. BFC的区域不会与`float box`叠加
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然
6. 计算BFC的高度时，浮动元素也参与计算

## 创建BFC的方法:

---

1. float 除了none以外的值
2. overflow 除了visible 以外的值（hidden，auto，scroll ）
3. display (table-cell，table-caption，inline-block, flex, inline-flex)
4. position值为（absolute，fixed）
5. fieldset元素



## 应用BFC的例子

---

#### 解决margin-collaps

```html
<style>
    p {
        color:black;
        background: #FF0000;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 50px;
    }
</style>
<p>
    hello world
</p>
<p>
    hello world
</p>
<p>
    hello world
</p>
```

在同一个BFC下，相邻box之间会发生margin-collapse，解决办法是使目标盒子在一个BFC新的BFC内，那么其外边距不会受到，这符合第5点 - BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然

```html
<style>
    .BFC {
        overflow: hidden;
    }
</style>

<div class="BFC">
    <p>
        hello, world!
    </p>
</div>
```



#### 用于布局，清除浮动

```html
<style>
    body {
        width: 300px;
        position: relative;
    }
.aside {
        width: 100px;
        height: 150px;
        float: left;
        background: blue;
     
}
     
 .main {
        height: 200px;
                background: #f00;
}
</style>

<div class="aside"></div>
<div class="main"></div>

```

BFC中每个元素的`margin box`会和包含快的`border box`接触，即使存在`float box` 导致`float box`会遮盖住元素，我们可以运用BFC规则 - **新的BFC不会和`float box`重叠**来解决问题

```css
.main {
    overflow: hidden;
}
```



#### 清除浮动，让包含块包含浮动元素

浮动元素无法撑开父元素容器就是因为浮动元素脱离父元素的包含块，产生塌陷。但是在BFC中 - **BFC计算浮动盒子的高度** ， 我们可以触发父元素容器BFC，从而使得浮动元素的高度能被计算

```html
<style>
.BFC {
        border: 5px solid #f00;
        width: 300px;
          overflow:hidden;
    }
 
    .box {
        border: 5px solid blue;
        width:100px;
        height: 100px;
        float: left;
    }
</style>

<div class="BFC">
    <div class="box">
         
    </div>
    <div class="box">
         
    </div>
</div>

```



#### BFC中元素不会被外部元素影响

<iframe width="100%" height="300" src="//jsrun.net/8khKp/embedded/html,css,result/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在上述例子中，紫色box中的little子元素被绿色`float box`影响了，要消除这个影响，可是在紫色box中创建BFC

```css
.right {
    overflow: hidden;
}
```





在实际中，利用BFC可以**闭合浮动**，**防止与浮动元素重叠**。同时，由于BFC的隔离作用，可以利用BFC包含一个元素，**防止这个元素与BFC外的元素发生margin collapse**。