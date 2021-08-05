---
title: ECMA Script6
date: 2021-06-30 23:20
tags: ES6
abbrlink: '0'
categories: ES6
---

### 一. let声明变量

```js
let a;
let b,c,d;
let e = 100;
let f = 521, g = 'iloveyou', h = [];
```

1. 变量不能重复声明

```js
let star = '罗志祥';
let star = '小猪';
// "star" has already been declared
```

2. 块级作用域

```js
{
  let girl = '周扬青';
}
console.log(girl);
// referenceError: girl is not defined
// if else while for循环
```

3. 不存在变量提升

~~~js
console.log(song);
var = song = '恋爱达人';
// Cannot access 'song' before initialization
~~~

4. 不影响作用域链

```js
{
  let school = 'peking';
  function fn(){
    console.log(school);
  }
  fn();
}
// peking
```

### 二. const声明常量

```js
const SCHOOL = '尚硅谷';
console.log(SCHOOL);
```

1. 一定要赋初始值

```js
const A;
// Missing initializer in const declaration
```

2. 一般常量使用大写(潜规则)

```js
const a = 100;
```

3. 常量的值不能修改

```js
SCHOOL = 'ATGUIGU';
// Assignment to constant variable
```

4. 块级作用域

```js
{
  const PLAYER = 'UZI';
}
console.log(PLAYER);
// PLAYER is not defined
```

5. 对于数组和对象的元素修改,不算做对常量的修改,不会报错

```js
const TEAM = ['UZI', 'MXLG', 'Letme'];
TEAM.push('Meiko');
```

### 三. 变量解构赋值

```js
// ES6 允许按照一定模式从数组和对象中提取值,对变量进行复制
// 这被称为解构赋值
// 1. 数组的解构
const F4 = ['小沈阳', '刘能', '赵四', '宋小宝'];
let [xiao, liu, zhao, song] = F4;
console.log(xiao);
console.log(liu);
console.log(zhao);
console.log(song);
//小沈阳
//刘能
//赵四
//宋小宝

// 2. 对象的解构
const zhao = {
  name: '赵本山',
  age: '不详',
  xiaopin: function(){
    console.log("我可以演小品");
  }
};

let {name, age, xiaopin} = zhao;
console.log(name);
console.log(age);
console.log(xiaopin);
xiaopin();
//赵本山
//不详
//f (){
// console.log("我可以演小品");
// }
// 我可以演小品
```

### 四. ES6引入新的声明字符串的方式 [``]  ' '  " "

```js
// 1. 声明
let str = `我也是一个字符串哦!`;
console.log(str, typeof str);
//我也是一个字符串哦! string

// 2. 内容中可以直接出现换行符
let str = `<ul>
						<li>沈腾<li>
    				<li>玛丽<li>
    				<li>魏翔<li>
    				<li>艾伦<li>
    				<ul>`;
// 3. 变量拼接
let lovest = '魏翔';
let out = `${lovest}是我心目中最搞笑的演员!!`;
console.log(out);
// 魏翔是我心目中最搞笑的演员!!
```

### 五. 简化对象写法

```js
// ES6 允许在大括号里面,直接写入变量和函数,作为对象的属性和方法
// 这样的书写更加简洁
let name = '尚硅谷';
let change = function(){
  console.log('我们可以改变你');
}

const school = {
  name, // name: name,
  change,
  improve: function(){
    console.log("我们可以提高你的技能");
  }
}

console.log(school);
```

### 六. 箭头函数

```js
// ES6允许使用[箭头] (=>)定义函数
// 声明一个函数
// let fn = function(){

// }
// 声明
let fn = (a, b) => {
  return a + b;
}
// 调用函数
let result = fn(1, 2);
console.log(result);

// 和之前区别:
// 1. this 是静态的. this 始终指向函数声明时所在作用域下的this的值
function getName(){
  console.log(this.name);
}
let getName2 = () => {
  console.log(this.name);
}
// 设置 window对象的name属性
window.name = '乐乐';
const school = {
  name: "LELE"
}

// 直接调用
getName();
getName2();
// 乐乐
// 乐乐

// call方法调用
getName.call(school);
getName2.call(school);
// LELE
// 乐乐

// 2. 不能作为构造实例化对象
let Person = (name, age) => {
  this.name = name;
  this.age = age;
}
let me = new Person('xiao', 30);
console.log(me);

// 3. 不能使用arguments变量
```

