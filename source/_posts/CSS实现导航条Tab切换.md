---
title: CSS实现导航条Tab切换
date: 2018-09-15 19:47:39
tags: [CSS, Font-end]
categories: 网络编程
---

# CSS实现导航条Tab切换

[源博客地址](https://www.cnblogs.com/xiaohuochai/p/6275647.html)

---

## 布局


模块整体叫做导航. 由导航标题盒导航内容组成，要实现上图所示布局效果，有两种布局方法：

+ 语义布局
+ 视觉布局

---

### 语义布局

```html
<style>
body,p{margin: 0;}
h2{margin: 0;font-size:100%;}
ul{margin: 0;padding: 0;list-style: none;}   
a{text-decoration: none;color:inherit;}
.box{width: 572px;border: 1px solid #999;overflow: hidden;}
.nav{margin-left: -1px;font: 14px "微软雅黑";overflow: hidden;background-color: #f1f1f1;}
.navI{float: left;width: 33.333%;box-sizing: border-box;}
.navI-tit{line-height: 40px;text-align: center;cursor: pointer;border-left: 1px solid #cecece;border-bottom: 1px solid #cecece;}
.navI-txt{width: 572px;height:200px;text-indent:2em;line-height: 2;background:#fff;}
.ml1{margin-left: -100%;}
.ml2{margin-left: -200%;}
.navI_active{position:relative;z-index:1;}
</style>

<div class="box">
    <ul class="nav">
        <li class="navI navI_active">
            <h2 class="navI-tit">课程</h2>
            <p class="navI-txt">课程内容</p>
        </li>
        <li class="navI">
            <h2 class="navI-tit">学习计划</h2>
            <p class="navI-txt ml1">学习计划内容</p>
        </li>
        <li class="navI">
            <h2 class="navI-tit">技能图谱</h2>
            <p class="navI-txt ml2">技能图谱内容</p>
        </li>
    </ul>   
</div>
```



`box-sizing: border-box`

|     值      |                             描述                             |
| :---------: | :----------------------------------------------------------: |
| content-box |         default。宽度和高度分别应用到元素的内容框。          |
| border-box  | 为元素设定的宽度和高度决定了元素的边框盒- 为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制 |
|   inherit   |            规定应从父元素继承 box-sizing 属性的值            |

`cursor: pointer`

| 值      | 描述                     |
| ------- | ------------------------ |
| url     | 需使用的自定义光标的 URL |
| default | 默认光标                 |
| auto    | 浏览器设置光标           |
| pointer | 呈现为指示链接的指针     |
| wait    | 显示程序正忙             |

Z-index 仅能在**定位元素**上奏效

简单来说. 



1. 做出横向的三个按钮
2. 对于按钮下的文本设置填充整个tag页面
3. 将第二，三个页面文本向左移动，和第一个文本重合
4. 设置`.navI_activate`页面`z-index`为1 （position也要设置为relative）



---

### 视觉布局

```html
<style>
body,p{margin: 0;}
ul{margin: 0;padding: 0;list-style: none;}
a{text-decoration: none;color: inherit;}
.box{width:572px;border:1px solid #999;font:14px "微软雅黑";overflow:hidden;}
.nav-tit{margin-left: -1px;height: 40px;line-height: 40px;text-align: center;background-color: #f1f1f1;overflow: hidden;}
.nav-titI{box-sizing: border-box;float: left;width: 33.333%;border-left: 1px solid #cecece;border-bottom: 1px solid #cecece;cursor: pointer;}
.nav-txt{height: 200px;text-indent: 2em; line-height: 2;}
.nav-txtI{height: 200px;}
</style>

<div class="box">
    <nav class="nav-tit">
        <a class="nav-titI">课程</a>
        <a class="nav-titI">学习计划</a>
        <a class="nav-titI">技能图谱</a>
    </nav>
    <ul class="nav-txt">
        <li class="nav-txtI nav-txtI_active">课程内容</li>
        <li class="nav-txtI">学习计划内容</li>
        <li class="nav-txtI">技能图谱内容</li>
    </ul>
</div>
```



1. 将`nav-tit`浮动布局形成宽度为1/3的按钮
2. `nav-txt`设置一定高度k
3. 将`nav-textI-active`设置成高度k

---

## hover 

导航条的功能就是点击导航标题时，显示对应的导航内容，如果使用:hover伪类实现类似效果，使用第一种布局方式-语义布局比较合适

---

### 语义布局

在语义布局中，三个导航内容处于重叠状态。

1. 移入父元素 `.navI`时，触发鼠标的hover态
2. 触发时使元素添加`positon: relative`和`z-index: 1`
3. 从而提升了子元素`p`层级`z-index` 父元素层级高则在重叠中显示在最上面

```css
.navI:hover {
    position: relative;
    z-index: 1;
}

.navI:hover .navI-tit {
    background: #fff;
    border-bottom: none;
}
```



缺点 : 

1. 初始状态时，第一个导航标题无法实现默认被选中的状态(背景白色，无下划线)；

2. 鼠标移出导航模块时，导航内容部分无法固定，显示第一个导航内容；

3. 鼠标移出导航模块时，导航标题的样式无法固定，恢复到默认状态


---

## 锚点

实现导航条的关键在于如何建立导航标题和导航内容之间的联系，而锚点就可以实现类似的效果。通过点击锚点，页面生成一个哈希值，然后跳转到相应的内容位置

---

### 使用语义布局

1. 利用锚点a指向#id的`navI-txt`
2. 设置:target使得z-index为1
3. 通过某种方式使得 `.navI-txt:target`能让`.navI-tit`变化

比较方便的办法是将`.navI-txt` 放在 `.navI-tit`上面，那么可以使用兄弟选择器设置`.navI-txt:target ~ .nav-tit`来改变相应的锚点的样式，那么需要将html进行重构

```html
<div class="box">
    <ul class="nav">
        <li class="navI navI_active">
            <p class="navI-txt" id="kc">课程内容</p>
            <a class="navI-tit" href="#kc">课程</a>
        </li>
        <li class="navI">
            <p class="navI-txt ml1" id="xx">学习计划内容</p>
            <a class="navI-tit" href="#xx">学习计划</a>
        </li>
        <li class="navI">
            <p class="navI-txt ml2" id="jn">技能图谱内容</p>
            <a class="navI-tit" href="#jn">技能图谱</a>
        </li>
    </ul>   
</div>
```



将上列的html样式变成原来的样式，就需要对`a`锚点设置绝对位置，对导航内容设置`margin-top`来避开`a`的内容

```css
body,p{margin: 0;}
h2{margin: 0;font-size:100%;}
ul{margin: 0;padding: 0;list-style: none;}   
a{text-decoration: none;color:inherit;}
.box{width: 572px;border: 1px solid #999;overflow: hidden;}
.nav{margin-left: -1px;font: 14px "微软雅黑";overflow: hidden;background-color: #f1f1f1;}
.navI{float: left;width: 33.333%;box-sizing: border-box;position:relative;}
.navI-tit{position:absolute;top:0;left:0;right:0;box-sizing: border-box;line-height: 40px;height: 40px;text-align: center;cursor: pointer;border-left: 1px solid #cecece;border-bottom: 1px solid #cecece;}
.navI-txt{width: 572px;height:200px;margin-top: 40px;text-indent:2em;line-height: 2;background:#fff;}
.ml1{margin-left: -100%;}
.ml2{margin-left: -200%;}
.navI_active{z-index:1;}
/*重点代码*/
.navI-txt:target{position:relative;z-index:1;}
.navI-txt:target ~ .navI-tit{background:#fff;border-bottom:none;}
```

需要注意的几点是

1. `positon: absolute；`是相对于已被定位的父元素来进行绝对定位
2. `z-index: 1；`也需要该元素进行定位，如果不要具体改动可以设置`position: relative`

缺点:

1. 初始态默认选中的导航标题样式无法设置；
2. 改变了HTML结构；
3. 锚点技术本身的局限是锚点目标会尽可能的到达可视区域上方，从而可能会生成页面跳动

其实在原有的界面上也可以实现

```css
.navI-tit:hover, .navI-tit:active {
  background: #fff;
  border-bottom: 1px solid white;
}
.navI-txt:target {
  position:relative;
  z-index: 1;
}
```

---

### 视觉布局

 在视觉布局中，**三个导航内容属于同一个父元素**，与父元素的高度相同，并按照块级元素的排列方式进行排布，**父元素设置溢出隐藏时，默认只显示第一个导航内容**

所以之前写好的视觉布局如果加上锚点，`navI-txt`中设置`overflow: hidden`自动满足标签页样式

下列可以让标签页样式更加酷炫

```css
. nav-txt {
    overflow: hidden;
  }
. nav-titI:hover {
  	background-color: white;
  	border-bottom: none;
  }

```

