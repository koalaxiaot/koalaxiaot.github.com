---
layout: post
title: ES6学习笔记-模板字符串
tag: 前端技术
---

### 旧方式

```javascript
var name = 'koala';
var output = 'my name is ' + name;
console.log(output); // my name is koala
```

### 模板字符串

> 使用`${var}`包裹变量，使用反引号``包裹字符串。

- 内嵌变量

```javascript
let name = 'koala';
let output = `my name is ${name}`;
console.log(output); // my name is koala
```

- 变量运算，并可执行函数。

```javascript
let x  = 1;
let y = 2;
let total = `total is : ${x + y}`;
console.log(total); // total is 3
```

```javascript
var helloFn = function(){
  return "hello world";
}
console.log(`${helloFn()}`); // hello world
```

- 多行字符串

```javascript
let str = `
 my name is 
 koala
`.trim();
console.log(str);
```
