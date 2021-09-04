---
title: JSON.stringify的一些事儿
date: 2021-09-04 13:40:29
tags:
---

前段时间面了某大厂，问了我好几个`JSON.stringify`的问题，我一时语塞，没答出来，结果就挂了。今天来总结一些关于`JSON.stringify`的一些知识点。

### 什么是JSON.stringify

> `JSON.stringify()` 方法将一个 JavaScript 对象或值转换为 JSON 字符串，如果指定了一个 replacer 函数，则可以选择性地替换值，或者指定的 replacer 是数组，则可选择性地仅包含数组指定的属性。

简单来讲就是把JS对象转化成JSON字符串。

### JSON.stringify的其他参数

`JSON.stringify()`的语法是这样的：`JSON.stringify(value[, replacer [, space]])`

第一个参数再熟悉不过，表示要转换的JS对象，无需赘述。

#### `replacer`

对于转换结果进行过滤或处理，可以为函数或者数组。默认是为空，当为`null`或者空的时候，不做任何处理，所有的属性都会序列化。

- **函数**
如果是函数，可以自定义方法对序列化结果进行处理，比如：值是`undefined`或`null`就返回空字符串

```js
const obj = { a: 1, b: undefined, c: null, d: { e: 'e1', f: 'undefined', g: null } }
const replacer = (k, v) => {
  if (v === undefined || v === null){
    return '';
  }
  return v
}
JSON.stringify(obj, replacer); 
// '{"a":1,"b":"","c":"","d":{"e":"e1","f":"undefined","g":""}}'
```

`replacer` 函数有两个参数，第二个参数代表值，第一个参数，如果序列化的对象是数组，表示**下标**，如果是对象表示对象的**key**，如果是非对象，则是**空字符串**。

- **数组**
如果是数组，则只有数组中包含的**属性名**才会被序列化。且成员的转换结果的顺序与属性名在数组中的顺序保持一致。只对序列化的值是对象类型起作用。

```js
JSON.stringify({a: 1, b: 2, c: 3}, ['c', 'a'])
// '{"c":3,"a":1}'
```

#### `space`

第三个参数指定缩进用的空白字符串，用于**美化输出**，可以是数字或字符串，默认为空表示没有空格；  
当`space` 是一个数字，表示缩进空格的个数，最多10个，小于1表示没有空格；  
当`space` 是一个字符串，用该字符串代替空格表示缩进，最多10个字符，超出取前10个 。

```js
JSON.stringify({a: 1, b: { c: 2, d: 3 }}, null, 3)
/** {
   "a": 1,
   "b": {
      "c": 2,
      "d": 3
   }
} **/

JSON.stringify({a: 1, b: { c: 2, d: 3 }}, null, '---')
/** {
---"a": 1,
---"b": {
------"c": 2,
------"d": 3
---}
} **/

```

### 一些特殊值的JSON.stringify

- `undefined`、函数会在序列化过程中会被忽略

```js
JSON.stringify({a: undefined, b: { c: 'c' , d: 3 }, e: () => {}})
// '{"b":{"c": "c", "d":3}}'
```

如果不希望被过滤，可以用上面提到的`replacer`处理一下

```js
JSON.stringify({a: undefined, b: { c: 'c' , d: 3 }, e: () => {}}, (k, v) => {
  if (v === undefined) {
    return 'undefined'
  }
})
// '{"a": "undefined", "b":{"c": "c", "d":3}}'
```

- 所有以 symbol 为属性键的属性都会被完全忽略掉，即便 `replacer` 参数中强制指定包含了它们
- 对象如果是循环引用的，会抛出错误

```js
let obj1 = {}; 
let obj2 = { b: obj1 }; 
obj1.a = obj2;
JSON.stringify(obj1);
/* VM5438:1 Uncaught TypeError: Converting circular structure to JSON
    --> starting at object with constructor 'Object'
    |     property 'a' -> object with constructor 'Object'
    --- property 'b' closes the circle
    at JSON.stringify (<anonymous>)
    at <anonymous>:1:6 **/
```

- 不可枚举的属性默认会被忽略

```js
let obj = {a : 'a', b: 'b'}
Object.defineProperty(obj, 'a', { enumerable: false }) // 将a属性设置为不可枚举
JSON.stringify(obj) // '{"b": "b"}'
```

- 布尔值、数字、字符串的包装对象会自动转换成对应的原始值

```js
JSON.stringify([new Number(1), new String("false"), new Boolean(true)]);
// '[1,"false",true]'
```

- `NaN` 和 `Infinity` 格式的数值会被当做 null

```js
JSON.stringify({a: NaN, b: Infinity}) // '{"a":null,"b":null}'
```

- 如果对象属性有 `toJSON` 方法，那么该方法就会替代默认的序列化行为

```js
JSON.stringify({a: 'a', b: 'b', toJSON: () => 'toJSON'});      // '"toJSON"'
```

## 参考

[MDN json.stringify](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
