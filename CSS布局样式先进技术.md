## CSS布局样式先进技术

#### 一、empty空元素的样式
* 1、`.xxx:empty { }` 伪类选择符`empty`
* 2、`.xxx:not(:empty) { }` 伪类选择符`not`

[空元素样式显示](https://jsbin.com/qerosinawo/1/edit?html,css,output)


#### 二、`xx_-of-type`伪类选择符 Pseudo-Classes Selectors
* 1、`first-of-type` 匹配同类型中的第一个同级兄弟元素.
* 2、`last-of-type` 匹配同类型中的最后一个同级兄弟元素.
* 3、`nth-of-type(n)` 匹配同类型中的第n个同级兄弟元素.
* 4、`only-of-type`
  * 一个层级只能一个该类型, 否则样式无效
  * 多层级生效
[xx_-of-type](https://jsbin.com/hupulucuza/1/edit?html,css,output)

#### 三、`calc`函数值来布局
* `width: calc(100% - 15rem);`

[calc函数](https://jsbin.com/yixebamoqe/2/edit?html,css,output)

#### 四、`vh`和`vw`实现纯`css`动态大小
* 1、`vh` 相对于视口的高度。视口被均分为100单位的vh
* 2、`vw` 相对于视口的宽度。视口被均分为100单位的vw

[vh和vw](https://jsbin.com/gedidaduro/2/edit?html,css,output)

#### 五、`vh`和`vw`的网页应用

[网页应用](https://jsbin.com/hupoyogami/1/edit?html,css,output)

#### 六、`unset`重置样式成上一层级，上一层级没设置该样式，默认的
* 重置成上一层级该属性值，上一层级没找到该属性值，重置成默认的

[unset](https://jsbin.com/falatalana/1/edit?html,css,output)

#### 七、`background-blend-mode` 混合模式


#### 八、响应布局的Columns列


[原文地址](https://egghead.io/courses/learn-advanced-css-layout-techniques)
