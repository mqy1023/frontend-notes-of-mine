
# 《其实你并不需要Lodash / Underscore》

接触前端较晚，主要使用的是React+ES5/ES6，下面大部分在前端开发过程中接触过，就当是做一份ES5/ES6笔记：

## Start

一、Array数组
#### 1、_.concat
将原数组拷贝一份副本, 拼接操作副本新数组,原数组不变

```javascript
const array = [1]
const other = array.concat(2, [3], [[4]])

console.log(other); // [1, 2, 3, [4]]
console.log(array); // [1]
```

#### 2、_.fill
修改替换数组中元素的值
```javascript
const array = [1, 2, 3];
array.fill('a');
console.log(array); // ['a', 'a', 'a']

console.log(Array(3).fill(2)); // [2, 2, 2]

console.log([4, 6, 8, 10].fill('*', 2, 3)); // [4, '*', '*', 10]; 从下标为2开始到下标为3,第三个为空时默认替换到数组最尾
```

#### 3、_.find
返回从数组中查找到符合条件的第一个值
```javascript
const users = [
    { 'user': 'barney',  'age': 36, 'active': true },
    { 'user': 'fred',    'age': 40, 'active': false },
    { 'user': 'pebbles', 'age': 1,  'active': true }
  ];
const findOut = users.find(item => item.age < 40);
console.log(findOut); // { active: true, age: 36, user: "barney" }
```

#### 4、_.findIndex
返回从数组中查找到符合条件的第一个值的下标，没有符合的返回-1
```javascript
const users = [
    { 'user': 'barney',  'age': 36, 'active': true },
    { 'user': 'fred',    'age': 40, 'active': false },
    { 'user': 'pebbles', 'age': 1,  'active': true }
  ];
const findIndex = users.findIndex(item => item.age < 40);
console.log(findIndex); // 0
```

#### 5、_.indexOf
返回从数组中查找到的第一个值的下标，没有符合的返回-1
Returns the first index at which a given element can be found in the array, or -1 if it is not present.
```javascript
const array = [2, 9, 9];
const result = array.indexOf(9);
console.log(result); // 1
```

#### 6、_.join
根据分隔符来拼接数组中元素
```javascript
const result = ['one', 'two', 'three'].join('--');
console.log(result); // "one--two--three"
```

#### 7、_.lastIndexOf
返回在数组中最后出现该值的index下标，没有返回-1
```javascript
const array = [2, 9, 9, 4, 3, 6];
const result = array.lastIndexOf(9);
console.log(result); // 2
```

#### 8、_.reverse
反转数组
```javascript
const array = [1, 2, 3];
console.log(array.reverse()); // [3, 2, 1]
```

**[⬆ back to top](#start)**

## 二、Collection集合
#### 1、_.each
依次yielding数组元素, 可以在中间操作每个元素
Iterates over a list of elements, yielding each in turn to an iteratee function.
```javascript
[1, 2, 3].forEach((value, index) => {
    console.log(value * 3);
}); // 3 6 9 依次输出
```

#### 2、_.every
数组中每个元素是否符合提供的测试条件函数
```javascript
const isLargerThanTen = (element, index, array) => {
   console.log(element, index, array);
   return element >= 10;
}

const array = [10, 20, 30];
const result = array.every(isLargerThanTen);
console.log(result); // true
```

#### 3、_.filter
过滤出符合提供的条件函数的值, 并返回它们组成的数组
```javascript
const isBigEnough = (value) => {
  return value >= 10
}
const array = [12, 5, 8, 130, 44];
const filtered = array.filter(isBigEnough);
console.log(filtered); // [12, 130, 44]
```

#### 4、_.includes
该值是否在数组中
```javascript
const array = [1, 2, 3];
array.includes(1); // true
// 用indexOf相同效果
const array = [1, 2, 3];
array.indexOf(1) > -1; // true
```

#### 5、_.map
将数组中元素每个值改变后返回新数组
```javascript
const array1 = [1, 2, 3];
const array2 = array1.map((value, index) => {
  return value * 2 + index;
});
console.log(array2); // [2, 5, 8]
```

**[⬆ back to top](#start)**

#### 6、_.pluck
pluck是Lodash v4.0之前的函数
```javascript
const array1 = [{name: "Alice"}, {name: "Bob"}, {name: "Jeremy"}];
const names = array1.map(x => {
  return x.name;
})
console.log(names); // ["Alice", "Bob", "Jeremy"]
```

#### 7、_.reduce
从左到右计算一个数组中各元素值
```javascript
const array = [0, 1, 2, 3, 4];
const result = array.reduce((previousVal, currentVal, currentIndex, array) => {
  return previousVal + currentVal;
});
console.log(result); // 等于各元素之和10
```

#### 8、_.reduceRight
从右到左计算一个数组中各元素值
```javascript
const array = [0, 1, 2, 3, 4];
const result = array.reduceRight((previousVal, currentVal, currentIndex, array) => {
  return previousVal - currentVal;
});
console.log(result); // 从右减到左，等于-2
```

#### 9、_.size
返回对象的键值对个数
```javascript
const result2 = Object.keys({one: 1, two: 2, three: 3}).length;
console.log(result2); // 3
```

#### 10、_.some
数组中是否有些元素符合条件
```javascript
const isLargerThanTen = (element, index, array) => {
  return element >= 10;
}
const array = [10, 9, 8];
const result = array.some(isLargerThanTen);
console.log(result); // true
```

**[⬆ back to top](#start)**

## 三、Function函数
#### 1、_.after
*【注】这只是一种可选的实现方式*在执行之前保证所有async calls异步回调已结束
```javascript
const notes = ['profile', 'settings'];
notes.forEach((note, index) => {
  console.log(note);
  if (notes.length === (index + 1)) {
    render(); // 到数组最后一个元素执行渲染方法
  }
});
```

## 四、Lang属性
#### 1、_.isNaN
检测值是否是NaN
```javascript
// ES6
console.log(Number.isNaN(NaN)); // true
```

**[⬆ back to top](#start)**

## 五、Object对象
#### 1、_.assign
将自身可enumerable属性加到目标对象
```javascript
const result1 = Object.assign({1: 'aaa'}, {2: 'bbb'});
console.log(result1); // { 1: "aaa", 2: "bbb" }

const result2 = Object.assign({1: 'aaa'}, ['a', 'b', 'c']);
console.log(result2); // { 0: "a", 1: "b", 2: "c" }
```

#### 2、_.keys
取出对象所有自身可enumerable属性
```javascript
const result2 = Object.keys({one: 1, two: 2, three: 3});
console.log(result2);  // ["one", "two", "three"]
```

**[⬆ back to top](#start)**

## 六、String字符串
#### 1、_.repeat
重复字符串count次
```javascript
const result = 'abc'.repeat(2);
console.log(result); // 'abcabc'
```

#### 2、_.toLower
字符串中字符所有大写转成小写
```javascript
const result = 'Fo1OBAR'.toLowerCase();
console.log(result); // "fo1obar"
```

#### 3、_.toUpper
字符串中字符所有小写转成大写
```javascript
const result = 'foobar'.toUpperCase();
console.log(result); // "FOOBAR"
```

#### 4、_.trim
裁剪字符串首尾的空符
```javascript
const result = ' abc '.trim()
console.log(result); // 'abc'
```

**[⬆ back to top](#start)**

## 【拓展】
#### 1、**unshift**向数组添加的第一个元素
```javascript
const data = ['1', '2', '3'];
data.unshift('0');

console.log(data); // ["0", "1", "2", "3"]
```

#### 2、**Array.from()**;
creates a new Array instance from an array-like or iterable object.
```javascript
Array.from({length: 5}, (v, index) => index); // [0, 1, 2, 3, 4]

Array.from([1, 2, 3], x => x + x); // [2, 4, 6]
```

**[⬆ back to top](#start)**
