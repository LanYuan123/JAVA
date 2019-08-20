### 规定链接的颜色：
   - a:link未访问的链接
   - a:visited已访问的链接
   - a:hover当有鼠标停留在链接上时
   - a:active已选择的链接
   
   注释：为了产生预期的效果，在 CSS 定义中，a:hover 必须位于 a:link 和 a:visited 之后！！

   注释：为了产生预期的效果，在 CSS 定义中，a:active 必须位于 a:hover 之后！！
   
### 设置CSS背景颜色渐变

background: -webkit-linear-gradient(top,颜色,颜色);

### 设置CSS背景透明度

background:rgba(255,0,0,1);

**RGBA 是代表Red（红色） Green（绿色） Blue（蓝色）和 Alpha（不透明度）三个单词的缩写。RGBA 颜色值是 RGB 颜色值的扩展，带有一个 alpha 通道 - 它规定了对象的不透明度。**

rgba（)里的值的介绍：

R：红色值。正整数 （0~255）

G：绿色值。正整数 （0~255）

B：蓝色值。正整数（0~255）

A：透明度。取值0~1之间

### 去掉ul中li标签前面的原点

list-style-type: none;

### 去掉a标签的下划线效果

text-decoration：none;

### 一些好看的配色

![好看的配色](https://github.com/LanYuan123/JAVA/blob/master/%E5%89%8D%E7%AB%AF/img/%E9%A2%9C%E8%89%B2.png)

### 元素溢出元素框时解决办法

overflow 属性规定当内容溢出元素框时发生的事情

值| 描述
--|:--:
visible | 默认值。内容不会被修剪，会呈现在元素框之外。
hidden | 内容会被修剪，并且其余内容是不可见的。
scroll | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
auto | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。

### 设置框四角变为圆角

border-radius:5px;

### CSS中的过渡

CSS transitions 提供了一种在更改CSS属性时控制动画速度的方法。 其可以让属性变化成为一个持续一段时间的过程，而不是立即生效的。比如，将一个元素的颜色从白色改为黑色，通常这个改变是立即生效的，使用 CSS transitions 后该元素的颜色将逐渐从白色变为黑色，按照一定的曲线速率变化。这个过程可以自定义。

**transition不支持display属性**

CSS transitions 可以决定哪些属性发生动画效果 (明确地列出这些属性)，何时开始 (设置 delay），持续多久 (设置 duration) 以及如何动画 (定义timing funtion，比如匀速地或先快后慢)。

### 设置网页title标签小图标

![title小图标](https://github.com/LanYuan123/JAVA/blob/master/%E5%89%8D%E7%AB%AF/img/title%E6%A0%87%E7%AD%BE.png)

**href：小标签图标的地址**

**size：图片的大小**

**size可以不要**
