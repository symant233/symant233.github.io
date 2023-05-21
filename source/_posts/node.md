---
title: Node.js笔记
date: 2020-06-04 18:16:50
tags:
  - node
  - javascript
categories:
---

### Module

CommonJS

```js
// file.js
const fs = require("fs"); // 导入
function myFunc() {}
exports.func = myFunc; // 导出
exports.valueOne = 123; // 导出
// 其他地方能用
const { func, valueOne } = require("./file");
// 文件只导出一个
class Fox {}
module.exports = new Fox();
// 这是导出一个新的实例 也可导出class再在导入的地方
```

ECMAScript 6

```js
// index.js
import { func, valueOne } from "./file";
export const Text = "aaa";
export default new Fox();
```

<!-- more -->

### MongoDB

本地导出与恢复。（默认导出到当前命令行下的`dump`文件夹）

```bash
# localhost
mongodump --db <DATABASE> [-o|--out <DIRNAME>]
mongorestore
```

mongodb atlas 集群备份和恢复

```
# atlas cluster
mongodump --host <REPLICA-SET-NAME/CLUSTER-SHARD-00-00,CLUSTER-SHARD-00-01,CLUSTER-SHARD-00-02> --ssl --username <ADMINUSER> --password <PASSWORD> --authenticationDatabase admin --db <DATABASE>
mongorestore --host <REPLICA-SET-NAME/CLUSTER-SHARD-00-00,CLUSTER-SHARD-00-01,CLUSTER-SHARD-00-02> --ssl --username <ADMINUSER> --password <PASSWORD> --authenticationDatabase admin
```

### Date

```javascript
let time
time = Date.now()
# 1586335565920
time = new Date(159000000000)
# Wed Jan 15 1975 14:40:00 GMT+0800
time.toJSON()
# "1975-01-15T06:40:00.000Z"
time.getTime()
# 159000000000
```

### JS deepClone or [`structuredClone()`](https://developer.mozilla.org/zh-CN/docs/Web/API/structuredClone)

```js
function deepClone(obj) {
  if (typeof obj !== "object" || obj === null) {
    return obj;
  }
  const clone = Array.isArray(obj) ? [] : {};
  for (let key in obj) {
    if (Object.prototype.hasOwnProperty.call(obj, key)) {
      clone[key] = deepClone(obj[key]);
    }
  }
  return clone;
}
```

### Array

```js
Object.keys(array).forEach((index) => {
  array[index];
});
for (let index in array) {
  array[index];
}
// 扩展运算符(spread)
const arr1 = [1];
const arr2 = [2, 3];
arr1.push(...arr2); // [1, 2, 3]
const arr3 = [...arr2, 5]; // [2, 3, 5]
```

- 拷贝: 可以使用数组实例的`concat`方法或扩展运算符`[...arr]`来深拷贝一个数组 (即数组更改互不影响), 但是如果数组里有对象元素则是浅拷贝 (更改对象内容会同步更改, 拷贝了对象的引用). 更多可参考[link](https://juejin.im/post/6844904197595332622).

```js
deepClone = (element) => {
  if (typeof element !== "object") return element;
  if (element === null) return null;
  return element instanceof Array
    ? element.map((item) => deepClone(item))
    : Object.entries(element).reduce(
        (pre, [key, val]) => ({ ...pre, [key]: deepClone(val) }),
        {}
      );
};
```

### Function

参数默认值

```js
func = function (
  url,
  {
    async = true,
    global = true,
    // ... more default config
  } = {}
) {
  // ... do stuff
};
```

Closure 闭包

```js
const myFunc = (function () {
  let t = 1;
  return function () {
    return t++;
  };
})();
```

### Number

js 中所有数字都存储成 64 位浮点数, 但所有按位运算都以 32 位二进制数执行.

二进制比特位移运算[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#%E6%8C%89%E4%BD%8D%E7%A7%BB%E5%8A%A8%E6%93%8D%E4%BD%9C%E7%AC%A6), [JavaScript 位运算符](https://www.w3school.com.cn/js/js_bitwise.asp)

- 符号位(Sign) 1bit (0 正数 1 负数)
- 指数位(Exponent) 32 位 8bit 64 位 11bit
- 尾数(Mantissa) 32 位 23bit 64 位 52bit

```js
(-1 >>> 0).toString(2);
// "11111111111111111111111111111111" 32位, 十进制 -1
(-1 >>> 1).toString(2);
// "01111111111111111111111111111111"
// 十进制 2147483647, 等于 2^31 - 1 (32位最大值)
1 << 31; // 左移31位
//  10000000000000000000000000000000
// 十进制 -2147483648, 等于-(2^31) (32位最小值)
```

二进制转十进制 `parseInt(110, 2); // 6`. 64 位最大可表示 2^53

数值精度: [wangdoc](https://wangdoc.com/javascript/types/number.html#%E6%95%B0%E5%80%BC%E7%B2%BE%E5%BA%A6).. 比特位转化：[IEEE-754 Floating Point Converter](https://www.h-schmidt.net/FloatConverter/IEEE754.html).

### Classes [ref](https://basarat.gitbook.io/typescript/future-javascript/classes#statics)

```ts
class FooBase {
  static a = 0;
  public x: number;
  private y: number;
  protected z: number;
}
class Child extends FooBase {
  constructor() {
    super();
  }
}

FooBase.a; // 可以访问, 所有FooBase类
Child.a; // 和它的子类都共享同一个值

// EFFECT ON INSTANCES
var foo = new FooBase();
foo.x; // okay
foo.y; // ERROR : private
foo.z; // ERROR : protected

// EFFECT ON CHILD CLASSES
class FooChild extends FooBase {
  constructor() {
    super();
    this.x; // okay
    this.y; // ERROR: private
    this.z; // okay
  }
}
```

### Buffer 🔨

`Buffer` 对象用于以字节序列的形式来表示二进制数据。 [文档](http://nodejs.cn/api/buffer.html#buffer_buffer)

```js

```

### ES6 🔨

不用`var`因其存在变量提升，`let`和`const`块级作用域，不能声明前调用。

变量解构赋值，如`[x, y] = [y, x]`，详见 [link](https://es6.ruanyifeng.com/#docs/destructuring)。

字符串 （unicode，字符串`for ... of`遍历，模板字符串）[link](https://es6.ruanyifeng.com/#docs/string)。 字符串转十六进制 Unicode [link](https://stackoverflow.com/questions/21647928/javascript-unicode-string-to-hex)

字符串对象的方法 - `includes`，`startsWith`，`endsWith`参数皆为字符串。
还有方法`padStart`，`padEnd`第一个参数为长度，第二参数为填充。
`trim`方法想必都不陌生，还有对应的`trimStart`和`trimEnd`。

```js
"1".padStart(3, "0"); // "001"
```

RegExp [ES6](https://es6.ruanyifeng.com/#docs/regex), [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

```js
Math.trunc(-4.9); // -4 去除小数部分
Math.cbrt(8); // 2 开立方根
Math.pow(Math.abs(8), 1 / 3);
Math.hypot(3, 4); // 5 (3的平方加上4的平方，等于5的平方)
```

函数: 使用箭头函数需要注意`this`作用域等 [MDN 使用注意点](https://es6.ruanyifeng.com/#docs/function#%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F%E7%82%B9), [MDN 不适用场合](https://es6.ruanyifeng.com/#docs/function#%E4%B8%8D%E9%80%82%E7%94%A8%E5%9C%BA%E5%90%88)

### Path

```js
__dirname; // 文件的文件夹绝对路径路径
__filename; // 文件的绝对路径
process.cwd(); // 执行时的终端工作路径 (同./)
```

使用内置库`path`，中文[文档](http://nodejs.cn/api/path.html)。

```js
const path = require("path");
path.parse("/dir1/dir2/file.txt");
// { root: '/', dir: '/dir1/dir2', base: 'file.txt', ext: '.txt', name: 'file' }
```

- [path.resolve()](http://nodejs.cn/api/path.html#path_path_resolve_paths) 方法会将路径或路径片段的序列解析为绝对路径。
- [path.join()](http://nodejs.cn/api/path.html#path_path_join_paths)将所有给定的`path`片段连接到一起, 规范化生成的路径.

### Promise 🔨

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fd58bb33270f46de819c2dbd98a8779c~tplv-k3u1fbpfcp-zoom-1.image)

传入函数中使用的`resolve(..)`, `reject(..)`会调用`.then()`里的函数.

```js
const promise = new Promise((resolve, reject) => {
  const res = "message";
  if (false) reject("error"); // 出现异常reject
  // throw new Error("error message");
  resolve(res);
})
  .then(
    (response) => {
      // success (in then)
      return response; // "message"
    },
    function (rejectMsg) {
      // rejected (in then)
      console.log(rejectMsg); // "error"
    }
  )
  .catch((error) => {
    console.log(error);
  });
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。

```js
promise.then(
  function (value) {
    // success
  },
  function (error) {
    // failure
  }
);
```

### yarn audit fix ?

```bash
# Generate the package-lock.json file without installing node modules
npm install --package-lock-only
# Fix the packages and update the package-lock.json file
npm audit fix
# Remove the yarn.lock file and import the package-lock.json file into yarn.lock
rm yarn.lock
yarn import
# Remove the package-lock.json file
rm package-lock.json
```
