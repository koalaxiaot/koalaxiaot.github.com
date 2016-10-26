---
layout: post
title: ES6学习笔记-let/const
tag: Javascript
---

### 块级作用域

相较于ES5的**全局作用域**和**函数作用域**，ES6多了**块级作用域**

1. 块级作用域可无限嵌套
2. 外层作用域无法读取内层作用域的变量
3. 作用域中可声明函数，同声明let变量。尽量使用函数表达式，避免直接函数声明

### let

1. 类似var，但let的变量声明仅在当前代码块中有效
2. 没有变量提升
3. 存在temporal dead zone，在代码块中，let之前，变量都属于不可调用区域
4. 不允许重复声明


```javascript
{
    let x = 1;
    var y = 2;
}

console.log(y);
console.log(x); // ReferenceError: x is not defined
```

### const

1. 声明一个只读常量，作用域/变量提升等与let相同
2. 必须赋值，不能只声明不赋值
3. 不可变的是 地址，而非 值

### 注意点

let/const 声明的全局变量，不属于顶层对象的属性。

在console中执行如下代码

```javascript
var x = 1;
window.x // 1

let y = 2;
window.y // undefined
```
