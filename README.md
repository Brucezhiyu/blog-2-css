## 《CSS 知识总结》
* 浏览器渲染原理
* CSS 动画的两种做法（transition 和 animation）
  
#### 1.浏览器渲染原理
1. 浏览器渲染过程
* 根据HTML构建HTML树(DOM)
* 根据CSS构建CSS树(CSSOM)
* 将两棵树合并成一棵渲染树(render tree)
* Layout布局(文档流，盒模型，计算大小和位置)
* Paint绘制(画颜色)
* Composite合成
2. 三种更新方式
第一种，过程全部走一遍
(div.remove() 会触发当前消失，其他元素relayout)
第二种，只跳过Layout
(改变背景颜色，直接repaint+composite)
第三种，跳过Layout和paint
(改变transform，只需composite)
tips：csstriggers.com 查找属性的渲染流程
3. css动画优化
###### css优化：
* 坚持使用 transform 和 opacity 属性更改来实现动画。
* 使用 will-change 或 translateZ 提升移动的元素。
* 避免过度使用提升规则；各层都需要内存和管理开销。

###### js优化
* 对于动画效果的实现，避免使用 setTimeout 或 setInterval，请使用 requestAnimationFrame。
将长时间运行的 JavaScript 从主线程移到 Web Worker。
* 使用微任务来执行对多个帧的 DOM 更改。
* 使用 Chrome DevTools 的 Timeline 和 JavaScript 分析器来评估 JavaScript 的影响。
#### 2.CSS 动画的两种做法（transition 和 animation）
**1. transform 4个常用功能**
   
  **位移translate**
```html
<div>Static</div>
<div class="moved">Moved</div>
<div>Static</div>
```
```css
div {
  width: 60px;
  height: 60px;
  background-color: skyblue;
}

.moved {
  transform: translateX(10px); /* 横向移动10px */
  background-color: pink;
}
```
常用写法：
transform:translateX() 水平移动 
transform:translateY() 纵向移动
transform:translateZ() 三维纵向移动
transform:translate(x,y) 水平和纵向移动
transform:translate3D(x,y,z)三维方向移动
**旋转rotate**
常用写法：
transform:rotateX() 水平旋转 
transform:rotateY() 纵向旋转
transform:rotateZ() 三维纵向旋转
transform:rotate(x,y) 水平和纵向旋转
transform:rotate3D(x,y,z)三维方向旋转
**缩放scale**
常用写法：
transform:scale() 整体放大，缩小
transform:scaleX() 水平放大，缩小 
transform:scaleY() 纵向放大，缩小
transform:scale(x,y) 水平和纵向放大，缩小
tips:
设置中心视点在1000px
```css
.wrapper{
    perspective:1000px;
}
```
**倾斜skew**
常用写法:
transform:skewX() 水平倾斜 
transform:skewY() 纵向倾斜
transform:skew(x,y) 水平和纵向倾斜

tips:4种功能可以组合使用
```css
.a{
    transform:scale(1.5) rotate(45deg);
    /*放大1.5倍的同时旋转45度 */
}
```
**2. transition过度的用法**
作用:补充关键帧
语法:
transition:属性名 时长 过渡方式 延迟
```css
transition:left 200ms linear 200ms;
```
过渡方式：
linear |ease | ease-in | ease-out | ease-out | ease-in-out | cubic-bezier | step-start | step-end | steps
可用逗号分隔两个不同属性:
```css
transition:left 200ms,top 200ms;
```
可以用all代表全部属性:
```css
transition:all 1s;
```
tips:
1.并不是所有属性都能过度，display:none => block 不能过度，一般改成visibility:hidden => visible。
2.必须要有起始
3.如果除了起始，还有中间点怎么办？ 
使用多次transform/使用animation(声明关键帧，添加动画)
**3. animation过度的用法**
语法:
animation:时长 | 过度方式 | 延迟 | 次数 | 方向 | 填充方式 | 是否暂停 | 动画名
时长:1s 或1000ms
过渡方式:
linear |ease | ease-in | ease-out | ease-out | ease-in-out | cubic-bezier | step-start | step-end | steps
次数:3或2.4或infinite(无限次)
方向:reverse | alternate | alternate-reverse
填充方式:
none | forwards | backwards | both
是否暂停:
pause | running

tips:如何让动画停在最后一帧？
(遇到问题搜:css animation stop at end)
```css
#demo.start{
    animation:xxx 1.5s forwards;
    /* xxx是名称，forwards用来让动画停在最后一帧 */
}
@keyframes xxx{
    0%{
        transform:none;
    }
    66.66%{
        transform:translateX(200px);
        /* 66.66%时移到左边200px处 */
    }
    100%{
        transform:translateX(200px) translateY(100px);
        /* 100%时移到下方100px处 */
    }
}
```
