# 《响应式web设计 —— HTML5和CSS3实战》笔记


## 一、响应式网页设计(RWD, Responsive Web Design)示例

* 1、一些响应式开发的站点
  * [http://blog.teamtreehouse.com/](http://blog.teamtreehouse.com/)
  * [http://2011.dconstruct.org/](http://2011.dconstruct.org/)
  * [http://mediaqueri.es/](http://mediaqueri.es/)

* 2、HTML5优点
  * 省时省力
    * 文档类型声明只需要  `<!DOCTYPE html>`
    * `js`和`css`链接可省略`type`属性(type="text/css" type="text/javascript")；
    ```css
    <script src="js/jquery-1.6.2.js"></script>

    <link rel="stylesheet" href="theme.css" />
    ```
  * 语义化标签元素
  ```css
  <header>
    <hgroup>
    <h1>....</h1>
    <h2>....</h2>
    </hgroup>
    <nav>
        <ul>
            <li>....</li>
            <li>....</li>
            <li>....</li>
        <li>....</li>    
        </ul>
    </nav>
  </header>
  <article>
    <section>....</section>
    <section>....</section>
    <section>....</section>
    <section>....</section>
  </article>
  <aside>....</aside>
  <footer>....</footer>
  ```

* 3、css3为响应式布局设计奠定基础

## 二、媒体查询：支持不同的视口
* 1、媒体查询；可以对设备特性（如视口宽度）设置特定的CSS样式
```css
    body {
        background-color: grey;
    }
    @media screen and (max-width: 960px) { /* 小于960px像素时执行下面代码 */
        body {
            background-color: red;
        }
    }
    @media screen and (max-width: 768px) {
        body {
            background-color: orange;
        }
    }
    @media screen and (max-width: 550px) {
        body {
            background-color: yellow;
        }
    }
    @media screen and (max-width: 320px) { /* 小于320px像素时执行下面代码 */
        body {
            background-color: green;
        }
    }
```
* 2、媒体查询用法
  * css2中通过`<link>`标签的`media`属性为样式表指定设备类型
  ```css
  <link rel="stylesheet" type="text/css" media="screen" href="screen-styles.css">
  ```
  * 纵向放置的显示屏才读取`portrait-screen.css`
  ```css
  <link rel="stylesheet" media="screen and (orientation: portrait)" href="portrait-screen.css" />
  ```
  * 限制显示屏宽度大于800像素才加载样式文件
  ```css
  <link rel="stylesheet" media="screen and (orientation: portrait) and (min-width: 800px)" href="800wide-portrait-screen.css" />
  ```
  * `projection`满足任意条件都加载样式文件
  ```css
   <link rel="stylesheet" media="screen and (orientation: portrait) and (min-width: 800px), projection" href="800wide-portrait-screen.css" />
  ```

## 三、拥抱响应式布局
* 1、将网页从固定布局修改为百分比布局，**目标元素宽度 = 上下文元素宽度 * 百分比**
* 2、文字大小用em替换px，现代浏览器默认文字大小16像素, `1em`等于`16px`
```css
font-size: 48px;
font-size: 3em;
```

* 3、设置阈值
```css
max-width: 1000px; /* 最大宽度为1000px */
min-width: 200px; /* 最小宽度为200px */
```

## 四、响应式设计中的HTML5

* 1、全新的语义化标签 `section`、`nav`、`article`、`aside`、`hgroup`、`header`、`footer`、`address`
* 2、文本级语义标签 `<b>`、`<em>`、`<i>`

## 五、CSS3：选择器、字体和颜色模式
* 1、css3 多栏效果
```css
column-count: 4;
column-width: 150px;

/* 栏位间隙和分割线 */
column-gap: 2rem;
column-rule: 1px dashed #ccc;
```
* 2、css3属性选择器
  * "^匹配开头"的属性选择器；匹配所有'alt'属性值以'film'开头的图片
  ```css
  img[alt^="film"] {
    border: 3px dashed #e15f5f;
  }
  ```

  * "*匹配包含"；`Element[attribute*="value"]`

  * "$匹配结尾"；`Element[attribute$="value"]`

* 3、css3结构伪类
  * :`last-child`、:`first-child`选择器
  ```css
  li: last-child { }
  ```
  * :`nth-child`选择器
  ```css
  li: nth-child(even) { }  /*2, 2n+3, odd*/
  ```css
  * 否定:`not`选择器
  ```css
  nav ul li:not(.internal) a { } /*除去internal类*/
  ```
* 4、自定义网页字体；使用`@font-face`嵌入网页字体

## 六、用CSS3创造令人惊艳的美
* 1、文字阴影
```css
text-shadow: 1px 1px #cccccc; /* 参数分别表示右侧阴影大小，下侧阴影大小，模糊距离）*/
```
* 2、盒阴影
  ```css
  box-shadow: 0px 3px 5px #444444; /* 水平偏移距离、垂直偏移距离、模糊半径、阴影颜色 */
  ```
  可以将`box-shadow`后的第一个参数指定为`inset`来设置内阴影
  ```css
  box-shadow: inset 0px 3px 5px #444444;
  ```
* 3、背景渐变
  * 线性渐变 `linear-gradient`
  * 径向渐变 `radial-gradient`
  * 重复渐变 `repeating-linear-gradient`
* 4、背景大小
  * `auto` 使用图片的原始大小
  * `cover` 按照原始图片的长宽比缩放图片以填充整个元素区域
  * `contain` 按照图片原始长宽比缩放图片以使较长的一边适应元素大小

## 七、CSS3过渡、变形和动画
* 1、过渡动画四个属性
  * transition-property 要过渡的属性名称，比如background-color或text-shadow或者all
  * transition-duration 定义过渡效果持续的时间
  * transition-timing-function 定义过渡期间速度如何变化(easing、linear、ease-in、ease-in-out、cubic-bezier)
  * transition-delay 可选，定义过渡开始前的延迟事件
  ```css
  #content a {
      transition-property: all;
      transition-duration: 1s;
      transition-timing-function: ease;
      transition-delay: 0s;
  }
  /* 简写 */
  transition: all 1s ease 0s;
  ```
* 2、CSS3中的2D变形 `transform`
  * scale：用来缩放大小
  * translate：在屏幕内移动元素
  * rotate：按照一定的角度旋转元素
  * skew：按照X和Y轴对元素进行斜切
  * matrix：将上述若干变形效果组合成单个声明，并允许你用像素精度来控制变形效果
  ```css
  a:hover {
    transform: scale(1.6);
    transform: translate(40px, 0);
  }
  ```
* 3、CSS3动画效果
使用`@keyframes`关键帧
```css
@keyframes warning {
  from {
    text-shadow: 0px 0px 4px #000000;
  }
  50% {
    text-shadow: 0 0 40px #000000;
  }
  to {
    text-shadow: 0 0 4px #000000;
  }
}
```

## 八、用HTML5和CSS3征服表单
* 1、表单中元素
  * placeholder 显示占位符文字
  * required 为确保表单必须输入该值
  * autofocus 自动聚焦
  * autocomplete 自动完成功能来帮助用户完成输入
* 2、新输入类型
email、number、url、tel、search、pattern、color、range
```
<input type="email" />
<input type="number"/>
<input type="url"/>
<input type="tel"/>
<input type="search"/>
<input pattern="([a-zA-Z])"/>
<input type="color"/>
<input type="range" min="1" max = "10" />
```
