# 选择器的权重和优先级

- 权重等级
    - 内联样式：style='xxx'，权值1000
    - ID选择器，#content，权值100
    - 类、伪类和属性选择器：.content、:hover、[attribute]，权值10
    - 元素选择器、伪元素选择器：div、p，权值1
    - 通用选择器（*）、子选择器（>）和相邻同胞选择器（+）：权值0

- 规则
    - 权值越大的选择器优先级越高
    - 相同权值的优先级遵循后定义覆盖前定义

# 盒模型

- 盒
    - padding：内边距
    - border：边框
    - margin：外边距

Q：盒模型的宽度如何计算

- 固定宽度的盒子
    - 内容宽度+border宽度+padding宽度+margin宽度：

```html

<div style="padding:10px; border:5px solid blue; margin: 10px; width:300px;">
    这里设置的width是内容宽度
</div>
```

Q：如何设置宽度为内容宽度+padding宽度+border宽度

```html
// 设置div的样式为box-sizing:border-box(bootstrap使用)
<style>
    * {
        box-sizing: border-box;
    }
</style>
```

- 充满父容器的盒子
    - 子容器的div默认display:block，宽度会充满整个父容器
    - 增大margin、border或者padding导致内容宽度无法再压缩，会导致父容器宽度被迫增加

- 纵向margin重叠
    - 连续的标签设置同样的margin：Xpx，标签间的的纵向距离为Xpx
    - 如果是Xpx和Ypx，Y>X，标签间的纵向距离为Ypx

# 浮动float

- 用途
    - [图片标签设置float后，图片内文字会环绕图片](./示例/图片内文本环绕效果.html)
      ![20220226165058](https://s2.loli.net/2022/02/26/I4Zj8q7WlF9BQ2h.png)
    - float+div可以实现类似table实现的网页布局

Q：为何float会导致父元素塌陷？

- float
    - 破坏性：被设置了float的元素会脱离文档流
    - 包裹性：设置float后div的宽度会自动调整为包裹住内容宽度，而不是撑满整个父容器；div的display没有发生变化（display:block）
    - 清空格：元素间的排列不再纵向紧密，而是横向紧密
      ![20220226171843](https://s2.loli.net/2022/02/26/z2QyYafpgrcHnv6.png)

Q：手写clearfix(清除浮动影响)

所有float元素的父容器，一般情况下都应该加clearfix。[点我查看效果](./示例/图片内文本环绕效果.html)

```html

<style>
    .clearfix:after {
        content: '';
        display: table;
        clear: both;
    }

    .clearfix {
        *zoom: 1; /* 兼容IE低版本 */
    }
</style>
<div class="clearfix">
    <img src="https://t.hk.uy/aP6b" style="width:300px;height:300px;float: left">
    <img src="https://t.hk.uy/aP6x" style="width:300px;height:300px;float: left">
    <img src="https://t.hk.uy/aP6y" style="width:300px;height:300px;float: left">
</div>
```

# 定位position

Q：relative、absolute和fixed分别依据谁来定位？

- position用于网页元素的定位
    - static：默认值，无需设置即可生效
    - relative：相对定位，导致自身位置的相对变化，而不会影响其他元素的位置、大小

  ```html
  <p>第一段文字</p>
  <p>第二段文字</p>
  <p>第三段文字</p>
  <p>第四段文字</p>
  
  // 增加position:relative，并设置left、top
  <p>第一段文字</p>
  <p>第二段文字</p>
  <p style="position:relative; top: 10px; left: 10px">第三段文字</p>
  <p>第四段文字</p>
  ```

    - absolute：绝对定位，
      - 导致元素脱离了文档结构 => 导致父元素坍塌
      - 包裹性
      - 跟随性
      - 悬浮于页面上方，遮挡下方页面内容
      - 定位依据最近的定位上下文（设置了position:relative/absolute/fixed的父元素），而不是相对于浏览器定位.
  
    - fixed：
      - 类似absolute、不同在于fixed根据window（或者iframe）确定位置

# flex 布局

```html
<style type="text/css">
    .container {
        display: flex;
    }
    .item {
        border: 1px solid #000;
        flex: 1
    }
</style>

<div class="container">
    <div class="item">aaa</div>
    <div class="item" style="flex: 2;">bbb</div>
    <div class="item">ccc</div>
    <div class="item">ddd</div>
</div>
```
![20220226205518](https://s2.loli.net/2022/02/26/JmsZ78YzqoUQ5Dy.png)

![20220226205816](https://s2.loli.net/2022/02/26/O8lKmZFohWHCSvu.png)

- 设置主轴的方向（flex-direction）
  - row（default）：水平方向，自左向右
  - row-reverse：水平方向，自右向左
  - column：垂直方向，自上向下
  - column-reverse：垂直方向，自下向上

- 设置主轴的对齐方式（justify-content）
  - flex-start（default）：向主轴开始方向对齐
  - flex-end：向主轴结束方向对齐
  - center；居中
  - space-between：两端对齐，项目之间的间隔都相等
  - space-around：每个成员两侧的间隔相等，头尾两个成员与容器的间隔是与其他成员之间的间隔的一半

- 交叉轴的对齐方式（align-items）
  - flex-start：交叉轴的起点对齐（向上对齐）
  - flex-end：交叉轴的终点对齐（向下对齐）
  - center：交叉轴的中点对齐（水平居中对齐）
  - baseline：每个成员的第一行文字的基线对齐（第一行文字水平对齐）
  - stretch（default）；未设置高度或者auto，将占满整个容器的高度

Q：如何实现居中对齐

- 水平居中
  - inline（行内元素）：text-align:center
```css
.container {
  text-align: center;
}
```
  - block（块元素）：margin:auto
```css
.container {
  text-align: center;
}

.item {
  width: 1000px;
  margin: auto;
}
```
  - 绝对定位元素：结合left和margin，但需要知道宽度
```css
.container {
  position: relative;
  width: 500px;
}

.item {
  width: 300px;
  height: 100px;
  position: absolute;
  left: 50%;
  margin: -150px;
}
```

Q：如何实现垂直居中？

- inline（行内元素）：设置line-height等于height的值
```css
.container {
  height: 50px;
  line-height: 50px;
}
```
- 绝对定位元素：结合left和margin，但要知道高度。优点：兼容性好；缺点：需要知道高度
```css
.container {
  position: absolute;
  height: 200px;
}
.item {
  width: 80px;
  height: 40px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-top: -20px;
  margin-left: -40px;
}
```
- 绝对定位结合transform。优点：不需要提前知道高度；缺点：兼容性不好
```css
.container {
  position: relative;
  height: 200px;
}

.item {
  width: 80px;
  height: 40px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: blue;
}
```
- 绝对定位结合margin:auto。优点：不需要知道高度，兼容性好
```css
.container {
  position: relative;
  height: 300px;
}

.item {
  width: 100px;
  height: 50px;
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

# 语义化

Q；如何理解HTML语义化？

- 语义：更易读懂
  - 让人（写程序、读程序）更易读懂
  - 让机器（浏览器、搜索引擎）更易读懂

# CSS3动画

- 使用@keyframes [animation-name]定义动画
- 通过百分比来设置不同的css样式，规定动画的变化
- 使用css选择器设置动画
```css
@keyframes selfAnimation {
  0% {background: red;left: 0;top: 0}
  25% {background: yellow;left: 200px;top: 0}
  50% {background: blue;left: 200px;top:200px}
  75% {background: green;left: 0;top: 200px}
  100% {background: red;left: 0;top: 0}
}

div {
  width: 100px;
  height: 50px;
  position: absolute;
  animation-name: selfAnimation;
  animation-duration: 5s;
}
```

- 配置项
  - animation-duration；动画时长
  - animation-name：动画名称
  - animation-timing-function：动画的速度曲线。默认是ease
  - animation-delay：动画何时开始。默认是0、
  - animation-iteration-count：动画被播放的次数。默认是1
  - animation-direction：动画是否在下一周期逆向地播放。默认是normal
  - animation-play-state：动画是否正在运行或暂停。默认running
  - animation-fill-mode；动画执行前和执行后如何给动画的目标应用，默认是none；保留在最后一帧可以使用forwards

Q：css的transition和animation有何区别？

- transition和animation都可以做动效
- transition是由一个状态过渡到另一个状态，动画可以由多个状态过渡组成
- animation有帧的概念，可以设置关键帧keyframe，动画可以由多个关键帧组成

# 重绘和回流

- 重绘：页面中的元素不脱离文档内流，而简单地进行样式的变化，比如修改颜色、背景等，浏览器重新绘制样式
- 回流：处于文档流中DOM的尺寸大小、位置或者某些属性发生变化时，导致浏览器重新渲染部分或全部文档的情况
- 回流比重绘更消耗性能开支。
- 一些属性的读取也会引起回流，比如读取某个DOM的高度和宽度，或者使用getComputedStyle方法
- 在编写代码的过程中要避免回流和重绘。

Q：指出下面代码的优化点，并且优化它

```javascript
var data = ['string1', 'string2', 'string3']
for(var i = 0; i< data.length; i++) {
    var dom = document.getElementById('list')
    dom.innerHTML += '<li>' + data[i] + '</li>'
}
```

上面的代码在循环中每次都获取dom，然后对其内部的HTML进行累加li，每次都会操作DOM结构，可以改成使用documnentFragment或者先遍历组成HTML的字符串，最后再操作一次innerHTML