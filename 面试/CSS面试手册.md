## CSS面试手册

### 盒模型

一个盒子由**content**、**padding**、**border**和**margin**组成。

根据计算宽高的区域分为四种模型：

-   content-box（标准盒模型）
-   border-box（IE盒模型）
-   padding-box（不支持）
-   margin-box（不支持）

但是现在w3c与mdn只支持前两种。

### 格式化上下文

格式化上下文说的是页面中的一块渲染区域，规定了渲染区域内部的子元素是如何排版及相互作用的，主要有四类：

-   BFC（Block Formatting Context）块级格式化上下文
-   IFC（Inline Formatting Context）行内格式化上下文
-   FFC（Flex Formatting Context）弹性格式化上下文
-   GFC（Grid Formatting Context）格栅格式化上下文

其中BFC和IFC较为重要，这里重点关注BFC。

**BFC**

块级格式化上下文，是一个独立的渲染区域，让处于BFC内部元素与外部元素相互隔离，使内外元素的定位不会相互影响。

**创建BFC条件**

-   根元素html
-   position：absolute/fixed
-   float元素
-   定义成块级的非块级元素display：inline-blok/flex/table-cell等
-   overflow不为visible

**渲染规则**

-   同一个BFC内部盒子垂直排列
-   同一个BFC内的两个盒子margin会重叠
-   计算BFC高度时，浮动元素也参与运算
-   BFC 中子元素的 margin 的左边， 与包含块 (BFC) border的左边相接触 (子元素 absolute 除外)
-   BFC的区域不会与float元素的区域重叠

**应用场景**

-   清除内部浮动
-   自适应两栏布局
-   防止margin重叠

### 层叠上下文

层叠上下文即为在三维空间中的Z轴。

**层叠等级**


![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d0138d43071e424c923b7827faeab2d1~tplv-k3u1fbpfcp-watermark.image)
### 居中布局

-   水平居中

    -   absolute + transform
    -   flex + justify-content: center
    -   行内元素：text-align: center
    -   块级元素：margin: 0 auto

<!---->

-   垂直居中

    -   line-height: height
    -   flex + align-items: center
    -   absolute + transform
    -   table

-   水平垂直居中

    -   absolute + transform
    -   flex + justify-content: center + align-items: center
### 选择器权重和优先级

权重：从0开始，一个行内样式+1000，一个id选择器+100，一个属性选择器、class或者伪类+10，一个元素选择器，或者伪元素+1，通配符+0

优先级：

-   权重相同，写在后面的覆盖前面的
-   使用！important达到最大优先级，都使用!important时，权重大的优先级高

### CSS伪类和伪元素

**伪类：**

-   :active，将样式添加到被激活的元素。
-   :focus，将样式添加到被选中的元素。
-   :hover，当鼠标悬浮在元素上方是，向元素添加样式。
-   :link，将特殊的样式添加到未被访问过的链接。
-   :visited，将特殊的样式添加到被访问的链接。
-   :first-child，将特殊的样式添加到元素的第一个子元素。
-   :lang，允许创作者来定义指定的元素中使用的语言。

**伪元素：**

-   :first-letter，将特殊的样式添加到文本的首字母。
-   :first-line，将特殊的样式添加到文本的首行。
-   :before，在某元素之前插入某些内容。
-   :after，在某元素之后插入某些内容。

### CSS预处理器原理

常见的有Sass/Less/Postcss。

原理：将类CSS语言通过Webpack编译转成浏览器可读的真正的CSS，给予了CSS更多强大的功能，常用功能有变量、嵌套、循环语句、条件语句、自动前缀、单位转换、mixin复用。

### 动画

#### transition和animation的区别

transition只有两个状态：开始状态和结束状态；但是animation可能是有多个状态，有帧的概念。

transition需要借助别的方式来触发，比如:hover或者借助JavaScript来触发；animation可以自动触发。

#### trasition过渡动画

transition属性是一个简写属性，用于设置四个过渡属性：

-   transition-property：属性
-   transition-duration：间隔
-   transition-timing-function：曲线
-   transition-delay：延迟

#### animation

animation 属性是一个简写属性，用于设置六个动画属性：

-   animation-name：动画名称
-   animation-duration：间隔
-   animation-timing-function：曲线
-   animation-delay：延迟
-   animation-iteration-count：次数
-   animation-direction ：方向

### 清除浮动

**什么是浮动：**

浮动元素会脱离文档流并向左/向右浮动，直到碰到父元素或者另一个浮动元素。

**问题：**

浮动元素会脱离正常的文档流，并不会占据文档流的位置。如果一个父元素下面都是浮动元素，那么这个父元素就无法被浮动元素所撑开，这样一来父元素就丢失了高度，这就是所谓的浮动造成的父元素高度坍塌问题。为了解决这个问题，所以需要清除浮动，让父元素恢复高度，方法如下：

**BFC清除浮动：** 通过给父元素设置overflow:hidden即可。

**clear清除浮动：**

利用:after生成一块内容为空的块级元素，然后通过clear 将这个伪元素移动到所有它之前的浮动元素的后面，例子如下：

设置前：


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/957bc67dae9c4093ac9472f4d484139c~tplv-k3u1fbpfcp-watermark.image)
```css
 .parent::after {
   content: "::after";
   ...
 }
 .clearfix::after {
   clear: both;
 }
```

设置后：


![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9bcd46c4bd134e63941fc328205d2f30~tplv-k3u1fbpfcp-watermark.image)
### 常用布局

#### 两栏布局


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8c080b0169434644bf5b3b70cb56a19b~tplv-k3u1fbpfcp-watermark.image)
```html
 <div class="layout">  
   <section>
     <aside class="orange">aside</aside>
     <main class="green">main</main>
   </section>
 </div>
```

**法一：float+overflow（BFC）**

```css
 aside{
   float:left;
   width:200px;
 }
 main{
   overflow:hidden;
 }
```

**法二：float+margin**

```css
 aside{
   float:left;
   width:200px;
 }
 main{
   margin-left:200px;
 }
```

**法三：flex**

```css
 .layout{
   display:flex;
 }
 aside{
   width:200px;
 }
 main{
   flex:1;
 }
```

**法四：grid**

```css
 .layout{
   display:grid;
   grid-template-columns:200px auto;
 }
```

### 三栏布局（两侧定宽主栏自适应）

#### 法一 圣杯布局

```html
 <div class="layout">
   <section>
     <main>main</main>
     <aside class="left">aside</aside>
     <aside class="right">aside</aside>
   </section>
 </div>
```

```css
 .layout{
   padding:0 200px
 }
 main{
   float:left;
   width:100%;
 }
 aside{
   float:left;
   width:200px;
 }
 .left{
   position:relative;
   left:-200px;
   margin-left:-100%;
 }
 .right{
   position:relative;
   right:-200px;
   margin-left:-200px;
 }
```

#### 法二 双飞翼布局

```html
 <section>
   <main><div class="inner">main</div></main>
   <aside class="left">aside</aside>
   <aside class="right">aside</aside>
 </section>
```

```css
 main{
   float:left;
   width:100%;
 }
 .inner{
   margin:0 200px;
 }
 aside{
   float:left;
   width:200px;
 }
 .left{
   margin-left:-100%;
 }
 .right{
   margin-right:-200px;
 }
```

#### 法三 float + overflow

```html
 <section>
   <aside class="left">aside</aside>
   <aside class="right">aside</aside>
   <main>main</main>
 </section>
```

```css
 aside{
   width:200px;
 }
 .left{
   float:left;
 }
 .right{
   float:right;
 }
 main{
   overflow:hidden;
 }
```

#### 法四 flex

```html
 <section class="flex">
   <aside>aside</aside>
   <main>main</main>
   <aside>aside</aside>
 </section>
```

```css
 .flex{
   display:flex;
 }
 aside{
   width:200px;
 }
 main{
   flex:1;
 }
```

### 自适应布局和响应式布局

-   自适应：需要开发多套界面。 通过检测视口分辨率，来判断当前访问的设备是：pc端、平板、手机，**从而请求服务层，返回不同的页面。**
-   响应式：只需要开发一套代码。 响应式设计通过检测视口分辨率，针对不同客户端在客户端做代码处理，来展现不同的布局和内容。

#### 响应式布局

**法一 媒体查询**

使用`@media`媒体查询可以针对不同的媒体类型定义不同的样式，特别是响应式页面，可以针对不同屏幕的大小，编写多套样式，从而达到自适应的效果。

```css
 @media screen and (max-width: 960px){
     body{
       background-color:#FF6699
     }
 }
 ​
 @media screen and (max-width: 550px){
     body{
       background-color:#00FF66;
     }
 }
```

**法二 百分比%**

当浏览器的宽度或者高度发生变化时，通过百分比单位，通过百分比单位可以使得浏览器中的组件的宽和高随着浏览器的变化而变化，从而实现响应式的效果。

height,width属性的百分比依托于父标签的宽高。但是，**padding、border、margin等属性的情况又不一样？**

-   子元素的top和bottom如果设置百分比，则相对于直接非static定位(默认定位)的父元素的高度，同样，子元素的left和right如果设置百分比，则相对于直接非static定位(默认定位的)父元素的宽度。
-   子元素的padding如果设置百分比，不论是垂直方向或者是水平方向，都相对于直接父亲元素的width，而与父元素的height无关。
-   子元素的margin如果设置成百分比，不论是垂直方向还是水平方向，都相对于直接父元素的width。
-   border-radius不一样，如果设置border-radius为百分比，则是相对于自身的宽度。

**法三 vw/vh**

css3中引入了一个新单位vw/vh，与视图窗口有关，vw表示相对于视图窗口的宽度，vh表示相对于视图窗口高度。 任意层级元素，在使用vw单位的情况下，1vw都等于视图宽度的百分之一。

**法四 rem**

rem单位是相对于字体大小的html元素，也称为根元素。 默认情况下，html元素的font-size为14px。所以此时1rem = 14px。

### 单行，多行文本溢出显示省略号

**单行**

```css
 .text {
   width: 100px;
   white-space: nowrap;
   overflow: hidden;
   text-overflow: ellipsis;
 }
```

**多行**

```css
 .text{
   width: 100px;
   overflow: hidden;
   text-overflow: ellipsis;
   display: -webkit-box;
   -webkit-line-clamp: 3;
   -webkit-box-orient: vertical;
 }
```
