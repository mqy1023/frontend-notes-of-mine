# Airbnb React/JSX Style Guide
可能是React/JSX最佳编码规范

## 基本规则(Basic Rules)
* 单个文件只包含一个React组件
* 单个文件可以有多个Stateless无状态组件组合，或多个纯组件
* 尽可能使用JSX语法
* 不要使用React.createElement，除非是从非jsx文件来初始化app

## 创建组件
#### Class vs React.createClass vs stateless
* 如果组件里面用到`state`状态 &/ `refs`，推荐使用`class extends React.Component`，除非有非得已的理由必须用到mixins时, 才选择用`React.createClass`
```jsx
// bad
 const Listing = React.createClass({
   // ...
   render() {
     return <div>{this.state.hello}</div>;
   }
 });

 // good
 class Listing extends React.Component {
   // ...
   render() {
     return <div>{this.state.hello}</div>;
   }
 }
```
* 如何组件中没有state和refs，推荐使用常规函数(非箭头函数)
```jsx
// bad
class Listing extends React.Component {
  render() {
    return <div>{this.props.hello}</div>;
  }
}

// bad (relying on function name inference is discouraged)
// 箭头函数自动绑定this函数本身, 这是有风险的, 所以不推荐用箭头函数写stateless组件
const Listing = ({ hello }) => (
  <div>{hello}</div>
);

// good
function Listing({ hello }) {
 return <div>{hello}</div>;
}
```

## 命名(Naming)
* 扩展名: React组件后缀使用`.jsx`
* 文件名：使用`PascalCase`命名规范，就是大写驼峰格式，如：ReservationCard.jsx
* 引用命名：组件使用`PascalCase`命名规范，组件实例用小写驼峰格式
```jsx
// bad
import reservationCard from './ReservationCard';

// good
import ReservationCard from './ReservationCard';

// bad
const ReservationItem = <ReservationCard />;

// good
const reservationItem = <ReservationCard />;
```
* 组件命名：文件名和组件名一致，如：`ReservationCard.jsx` 文件中组件名为`ReservationCard` ；然后一个文件目录下的`root` 根组件使用`index.jsx`来命名，同时以文件目录来命名该根组件
```jsx
// bad
import Footer from './Footer/Footer';

// bad
import Footer from './Footer/index';

// good
import Footer from './Footer';
```

> 使用场景1：
> ```jsx
>  . /Common/
>            ./Loading.jsx   => export { Loading }
>            ./Error.jsx     => export { Error }
>            ./index.jsx   => export * from './Loading';
>                             export * from './Error';
> import { Loading, Error } from './Common';
> import Common from './Common'; // 使用时用Common.Loading
> ```

* 高阶组件名：
[理解高阶组件可参考](https://github.com/mqy1023/react-with-es6/blob/master/03%E3%80%81components/src/jsx/HocApp.jsx)

* 属性命名：避免使用DOM组件的原有属性名
```jsx
// bad
<MyComponent style="fancy" />

// good
<MyComponent variant="fancy" />
```
## 声明组件(Declaration)
```jsx
// bad
export default React.createClass({
  displayName: 'ReservationCard',
  // stuff goes here
});

// good
export default class ReservationCard extends React.Component {

}
```

## 代码对齐(Alignment)
```jsx
// bad
<Foo superLongParam="bar"
     anotherSuperLongParam="baz" />

// good
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
/>

// if props fit in one line then keep it on the same line
<Foo bar="bar" />

// children get indented normally
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
>
  <Quux />
</Foo>
```

## Quotes(引号)
只有JSX属性用""双引号，其他情况下用单引号
> 为什么？常规HTML属性也是使用的是""

```jsx
// bad
<Foo bar='bar' />

// good
<Foo bar="bar" />

// bad
<Foo style={{ left: "20px" }} />

// good
<Foo style={{ left: '20px' }} />
```
## 空格(Spacing)
* 自关闭标签前要空出一个空格
```jsx
// bad
<Foo/>

// very bad
<Foo                 />

// bad
<Foo
 />

// good
<Foo />
```
* 不要在JSX花括号 {} 两边加空格.
```jsx
// bad
<Foo bar={ baz } />

// good
<Foo bar={baz} />
```
## 属性(Props)
* JSX属性名使用小写骆驼式
```jsx
// bad
<Foo
  UserName="hello"
  phone_number={12345678}
/>

// good
<Foo
  userName="hello"
  phoneNumber={12345678}
/>
```
* **属性的值为true时可省略**
```jsx
// bad
<Foo
  hidden={true}
/>

// good
<Foo
  hidden
/>
```
* `img`标签一定要有`alt`属性，或必须有`role="presentation"`
```jsx
// bad
<img src="hello.jpg" />

// good
<img src="hello.jpg" alt="Me waving hello" />

// good
<img src="hello.jpg" alt="" />

// good
<img src="hello.jpg" role="presentation" />
```
* 不要在 alt 值里使用如 "image", "photo", or "picture"包括图片含义这样的词
> 为什么? html解析器已经把 img 标签标注为图片了, 所以没有必要再在 alt 里说明了.

```jsx
// bad
<img src="hello.jpg" alt="Picture of me waving hello" />

// good
<img src="hello.jpg" alt="Me waving hello" />
```
* 使用有效正确的 aria role属性值 [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions).
```jsx
// bad - not an ARIA role
<div role="datepicker" />

// bad - abstract ARIA role
<div role="range" />

// good
<div role="button" />
```
* 不要在标签上使用 accessKey 属性.
> 为什么? html解析器在键盘快捷键与键盘命令时造成的不统一性会导致阅读性更加复杂.

```jsx
// bad
<div accessKey="h" />

// good
<div />
```
* **避免使用数组的index来作为属性key的值，推荐使用唯一ID**.[(why?)](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318#.s208dgej3)
```jsx
// bad
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}

// good
{todos.map(todo => (
  <Todo
    {...todo}
    key={todo.id}
  />
))}
```
* **对于非required属性要有defaultProps默认属性值**
```jsx
// bad
function SFC({ foo, bar, children }) {
  return <div>{foo}{bar}{children}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
  children: PropTypes.node,
};

// good
function SFC({ foo, bar }) {
  return <div>{foo}{bar}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
};
SFC.defaultProps = {
  bar: '',
  children: null,
};
```
## 引用(Refs)
```jsx
// bad
<Foo
  ref="myRef"
/>

// good
<Foo
  ref={(ref) => { this.myRef = ref; }}
/>
```
## Parentheses
当JSX标签超过一行时，要将其包裹在圆括号()中
```jsx
// bad
render() {
  return <MyComponent className="long body" foo="bar">
           <MyChild />
         </MyComponent>;
}

// good
render() {
  return (
    <MyComponent className="long body" foo="bar">
      <MyChild />
    </MyComponent>
  );
}

// good, when single line
render() {
  const body = <div>hello</div>;
  return <MyComponent>{body}</MyComponent>;
}
```
## 标签(Tags)
* 对于没有子元素的标签，使用自闭格式
```jsx
// bad
<Foo className="stuff"></Foo>

// good
<Foo className="stuff" />
```
* 如果模块有多行的属性， 新建一行关闭标签
```jsx
// bad
<Foo
  bar="bar"
  baz="baz" />

// good
<Foo
  bar="bar"
  baz="baz"
/>
```
## 方法(Methods)
* 使用箭头函数来掩盖当前变量
```jsx
function ItemList(props) {
  return (
    <ul>
      {props.items.map((item, index) => (
        <Item
          key={item.key}
          onClick={() => doSomethingWith(item.name, index)}
        />
      ))}
    </ul>
  );
}
```
* **在constructor构造函数中绑定render方法中用到的事件处理函数**
> 为什么? 在每次 render 过程中， 再调用 bind 都会新建一个新的函数，浪费资源.

```jsx
  // bad
  class extends React.Component {
    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv.bind(this)} />
    }
  }

  // good
  class extends React.Component {
    constructor(props) {
      super(props);

      this.onClickDiv = this.onClickDiv.bind(this);
    }

    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv} />
    }
 }
```
* **不要使用_前缀来命名内部方法**
> 为什么？_ 下划线前缀在某些语言中通常被用来表示私有变量或者函数。但是不像其他的一些语言，在JS中没有原生支持所谓的私有变量，所有的变量函数都是共有的。尽管你的意图是使它私有化，在之前加上下划线并不会使这些变量私有化，并且所有的属性（包括有下划线前缀及没有前缀的）都应该被视为是共有的。了解更多详情请查看Issue [#1024](https://github.com/airbnb/javascript/issues/1024), 和 [#490](https://github.com/airbnb/javascript/issues/490) 。

```jsx
  // bad
  React.createClass({
    _onClickSubmit() {
      // do stuff
    },

    // other stuff
  });

  // good
  class extends React.Component {
    onClickSubmit() {
      // do stuff
    }

    // other stuff
  }
```
*  确保 render 方法中总是return返回值
```jsx
// bad
render() {
  (<div />);
}

// good
render() {
  return (<div />);
}
```
## 组件的生命周期(Ordering)
* `class extends React.Component` 的生命周期函数:
	* 1、可选的`static` 静态方法
	* 2、`constructor`
	* 3、`getChildContext`
	* 4、`componentWillMount` ...
【余下生命周期可参考】(https://github.com/mqy1023/react-with-es6/tree/master/06%E3%80%81lifecyclemethods)
```jsx
import React, { PropTypes } from 'react';

const propTypes = {
  id: PropTypes.number.isRequired,
  url: PropTypes.string.isRequired,
  text: PropTypes.string,
};

const defaultProps = {
  text: 'Hello World',
};

class Link extends React.Component {
  static methodsAreOk() {
    return true;
  }

  render() {
    return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
  }
}

Link.propTypes = propTypes;
Link.defaultProps = defaultProps;

export default Link;
```
<hr />
#### 对于上面这么多最佳编码规范，在开发中当然不能完全自觉顾及到，需要加入静态检查代码，对于不好的编码实时提示，当然有很多种方式，如下只是一种参考：
1、`npm install --save-dev eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react`
2、在工程目录下新建`.eslintrc` 配置规则
```jsx
{
    "extends": "airbnb",
    "env": {
        "node": true,
    },
    "ecmaFeatures": {
        "forOf": true,
        "jsx": true,
        "es6": true
    },
    "rules": {
        "comma-dangle": 0,
        "react/prop-types": 0,
        "react/forbid-prop-types": 0,
        "max-len": ["warn", 120],
        "no-floating-decimal": 0
    }
}
```
上面省略了很多rules, 毕竟`airbnb`限制得太多太死，主要是用于取消`airbnb` 一些规则，在实战中根据提示扩充。
<hr / >

[《javascript/react》原文链接](https://github.com/airbnb/javascript/tree/master/react)
