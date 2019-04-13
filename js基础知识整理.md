#### 《一》、js基础

##### 一、几道面试题
* 1、js中使用typeof能得到的哪些类型（js变量类型）

（undefined, number, string, boolean）属于值类型

函数、数组, 对象、null、new Number(5)都是对象。他们都是引用类型。

```javascript
// 值类型
var a = 100
var b = a
a = 200
console.log(b) // 100

// 引用类型
var a = { age: 20 }
var b = a
b.age = 30
console.log(a.age) // 30

// typeof 只能区分值类型的详细类型
// typeof 区分引用类型只能区分出函数
typeof undefined // undefined
typeof 'abc' // string
typeof 123 // number
typeof true // boolean

typeof {} // object
typeof [] // object
typeof null // object
typeof console.log // function
```

* 2、何时使用===何时使用==？（强制类型转换）
```javascript
100 == '100'  // true
0 == '' // true
null == undefined // true

console.log(10 && 0) // 0
console.log('' || 'abc') // 'abc'
console.log('1' || 'abc') // '1'
console.log(!window.abc) // true

// 判断一个变量会被当成true还是false
var a = 100;
console.log(!!a) // true

// 何时使用==
if (obj.a == null) {
  // 这里相当于 obj.a === null || obj.a === undefined，此写法为jquery推荐
}
```

#### 《二》、原型和原型链

##### 一、构造函数
* 1、描述new一个对象的过程
  - 创建一个新对象
  - this指向这个新对象
  - 执行代码，即对this赋值
  - 返回this
```javascript
function Foo(name, age) {
  this.name = name
  this.age = age
  thia.class = 'class-1'
  // return this  // 默认有这一行
}
var f = new Foo('zhangsan', 20)
// var f1 = new Foo('lisi', 22) // 创建多个对象
```
##### 二、构造函数 - 扩展
* var a = {} 其实是var a = new Object() 的语法糖
* var a = [] 其实是var a = new Array() 的语法糖
* function Foo() {} 其实是var Foo = new Function()
* 使用instanceof 判断一个函数是否是一个变量的构造函数
  - 变量 instanceof Array, 判断一个变量是否为数组

##### 三、原型规则和示例
所有的引用类型（数组、对象、函数）都具有对象特性，即可自由扩展属性（除了null之外）
所有的引用类型（数组、对象、函数）都有一个`__proto__`(隐式原型)属性,属性值是一个普通的对象
所有的引用类型（数组、对象、函数）`__proto__`(隐式原型)属性值指向它的构造函数的`prototype`属性值

```javascript
var obj = {}
console.log(obj.__proto__)

var obj1 = [] // [object Object] { ... }
console.log(obj1.__proto__) // []

function fn () {}
fn.a = 100
console.log(fn.__proto__) // function () { [native code] }

console.log(fn.prototype) // [object Object] { ... }

console.log(obj.__proto__ === Object.prototype) // true
```
当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的`__proto__`(即构造函数的prototype)中寻找
```javascript
// 构造函数
function Foo(name, age) {
  this.name = name
}
Foo.prototype.alertName = function() {
  alert(this.name)
}
// 创建示例
var f = new Foo('zhangsan')
f.printName = function() {
  console.log(this.name)
}
// 测试
f.printName()
f.alertName()
```
循环自身的属性
```javascript
var item
for (item in f) {
  // 高级浏览器已经在 for in 中屏蔽了来自原型的属性
  // 但是建议这里加上这个判断，保证程序健壮性
  if (f.hasOwnProperty(item)) {
      console.log(item)
  }
}
```
##### 四、原型链
instanceof用于判断引用类型属于哪个构造函数的方法

* 1、如何判断一个变量是数组类型
```javascript
var arr = []
arr instanceof Array
```

* 2、写一个原型链继承的例子
```javascript
// 动物
function Animal(){
  this.eat = function() {
    console.log('animal eat')
  }
}
// 狗
function Dog(){
  this.bark = function() {
    console.log('dog bark')
  }
}
Dog.prototype = new Animal()
// 哈士奇
var hashiqi = new Dog()
```

* 4、写一个封装DOM查询的例子
```javascript
function Elem(id){
    this.elem = document.getElementById(id)
}
Elem.prototype.html = function (val){
    var elem = this.elem
    if (val) {
        elem.innerHTML = val
        return this;//链式操作
    } else {
        return elem.innerHTML
    }
}

Elem.prototype.on = function(type, fn){
    var elem = this.elem
    elem.addEventListener(type, fn)//用于向指定元素添加时间句柄
    return this;
}

var div1 = new Elem('div1')//把对应id写在这里
// console.log(div1.html())

div1.html('<p>hello World</p>').on('click',function(){//链式操作
    alert('clicked')
}).html('<p>javascript</P>')
```

#### 《三》、作用域和闭包

声明变量提升
```javascript
fn1("aa")
function fn1(name) {
  console.log(name)  // "aa"
}


fn2("aa")  //"TypeError: fn2 is not a function
var fn2 = function(name) {
  console.log(name)
}
```
this 要在执行时才能确认值，定义时无法确认
```javascript
var a = {
  name: 'aa',
  fn: function() {
    console.log(this.mame)
  }
}
a.fn() // this === a
a.fn.call({name: 'bb'})   // this === {name:'bb'}
var fn1 = a.fn;
fn1()  // this === window
```
js无块级作用域
```javascript
if (true) {  // 有和没有都一样效果，相当于var name在外面
  var name = 'zhangsan'
}
console.log(name)
```

* 4、用js创建10个<a>标签，点击的时候弹出来对应的序号？（作用域）
```javascript
for(var i=0;i<10;i++){
    (function(i){
        var a = document.createElement('a');
        a.innerHTML = i + '<br>';
        document.body.appendChild(a);
        a.addEventListener('click',function(e) {
            e.preventDefault();  //取消默认事件，指a标签
            alert(i);
        });
    })(i);
}
```
* 5、说一个闭包在实际开发中的应用
```javascript
function isFirstLoad(){
    var _list = [];
    return function(option) {
        if (_list.indexOf(option) >= 0) { //检测是否存在于现有数组中，有则说明已存在
            console.log('已存在')
            return false;
        } else {
            _list.push(option);
            console.log('首次传入'); //没有则返回true,并把这次的数据录入进去
            return true;
        }
    }
}

var ifl=isFirstLoad();
ifl("zhangsan");
ifl("lisi");
ifl("zhangsan");
```


#### 《三》、事件和跨域

* 1、编写一个通用的事件监听函数
```javascript
function bindEvent(elem, type, selecter, fn) {
    if (fn == null) {
        //判断一下selecter这个参数是否存在
        fn = selecter
        selecter = null
    }
    elem.addEventListener(type, function (e) {
        //代理
        if (selecter) {
            //代理
            var target = e.target
            if (target.matches(selecter)) {
            //这里target代表出发事件的节点，是否和你传的selecter相匹配
                fn.call(target, e)
            }
        } else {
            //
            fn(e)
        }
    })
}
```
我希望给div1里面的a标签绑定事件，但div1里的其他标签元素不绑定事件，注意此时是需要代理的
```javascript
var div1 = document.getElementById('div1')
bindEvent(div1, 'click', 'a',  function (e) {
    e.preventDefault()
    console.log(this.innerHTML)
})
```
我希望给id为p1的p标签绑定事件
```javascript
bindEvent(p1, 'click',  function (e) {
    // e.preventDefault()
    console.log('666')
})
```

* 2、对于一个无限下拉加载的页面，如何给每个图片绑定事件

##### JSONP技术原理及实现
它并不是ajax，它是在文档中插入一个script标签，创建_callback方法，通过服务器配合执行_callback方法，并传入一些参数
```javascript
<script>
window.callback = function(data) {
  console.log(data)  // 这是我们跨域得到的信息
}
</script>
<script src="http://xxx.yyy.com/api.js"></script>
<!-- 以上将返回callback(abc) -->
```

##### 1、浏览器加载一个资源的过程
* 1、浏览器更加DNS服务器得到域名的ip地址
* 2、想这个ip的机器发送http请求
* 3、服务器收到、处理并返回http请求
* 4、浏览器得到返回内容

##### 2、浏览器渲染页面的过程
* 1、根据html结构生成dom树
* 2、根据css生成cssom
* 3、将dom和cssom整合成RenderTree
* 4、根据RenderTree开始渲染和展示
* 5、遇到<script>时，会执行并阻塞渲染

#####  3、window.onload和DOMContentLoaded的区别？（浏览器渲染过程）
  - window.onload // 页面全部资源加载完才会执行，包括图片、视频等
  - DOMContentLoaded 渲染完即可执行，此时图片、视频可能还没有加载完

##### 4、防止xss攻击
  - 前端替换关键字，例如替换`< 为&lt; > 为&gt;`
  - 后端替换
