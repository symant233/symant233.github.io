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

### Array

```js
Object.keys(array).forEach((index) => {
  array[index];
});
// 扩展运算符(spread)
const arr1 = [1];
const arr2 = [2, 3];
arr1.push(...arr2); // [1, 2, 3]
const arr3 = [...arr2, 5]; // [2, 3, 5]
```

- 拷贝: 可以使用数组实例的`concat`方法或扩展运算符`[...arr]`来深拷贝一个数组 (即数组更改互不影响), 但是如果数组里有对象元素则是浅拷贝 (更改对象内容会同步更改, 拷贝了对象的引用). 更多可参考[link](https://juejin.im/post/5b5dcf8351882519790c9a2e).

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

### Number

js 中所有数字都存储成 64 位浮点数, 但所有按位运算都以 32 位二进制数执行.

二进制比特位移运算[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#%E6%8C%89%E4%BD%8D%E7%A7%BB%E5%8A%A8%E6%93%8D%E4%BD%9C%E7%AC%A6), [JavaScript 位运算符](https://www.w3school.com.cn/js/js_bitwise.asp)

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

## ES6

不用`var`因其存在变量提升，`let`和`const`块级作用域，不能声明前调用。

变量解构赋值，如`[x, y] = [y, x]`，详见 [link](https://es6.ruanyifeng.com/#docs/destructuring)。

字符串 （unicode，字符串`for ... of`遍历，模板字符串）[link](https://es6.ruanyifeng.com/#docs/string)。

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

未完成 待更新
