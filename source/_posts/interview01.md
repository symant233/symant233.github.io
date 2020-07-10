---
title: 遇到的面试题01
date: 2020-07-10 22:36:50
tags:
  - interview
categories:
  - interview
---

### part1

**1. http header** method 有哪些
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)

**通用首部** (General header): 同时适用于请求和响应消息，但与最终消息主体中传输的数据无关的消息头。

- `Connection`: 决定当前的事务完成后，是否会关闭网络连接。如果该值是“keep-alive”，网络连接就是持久的，不会关闭，使得对同一个服务器的请求可以继续在该连接上完成。
- `Date`: 通用首部，其中包含了报文创建的日期和时间。
- `Cache-Control`: 通过指定指令来实现缓存机制。详细参考[链接](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control) ✔

**请求头**(Request headers): 包含更多有关要获取的资源或客户端本身信息的消息头。某些请求头如 `Accept`、`Accept-*`、 `If-*` 允许执行条件请求。

<!-- more -->

```
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/testpage.html
Connection: keep-alive
Cookie: name=value; name2=value2; name3=value3
Upgrade-Insecure-Requests: 1
If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
Cache-Control: max-age=0
```

`Accept`: 用户代理期望的 MIME 类型列表. `Accept-Encoding`: 列出用户代理支持的压缩方法。
`DNT`: 设置该值为 1，表明用户明确退出任何形式的网上跟踪(Do Not Track)。

`Referer`: 包含了当前请求的来源页面的地址. 在以下两种情况下，`Referer` 不会被发送：

1, 来源页面采用的协议为表示本地文件的 "file" 或者 "data" URI；
2, 当前请求页面采用的是非安全协议，而来源页面采用的是安全协议（HTTPS）。

**响应头**(Response headers): 包含有关响应的补充信息.

```
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
```

`ETag`: 资源的特定版本的标识符。这可以让缓存更高效，并节省带宽，因为如果内容没有改变，Web 服务器不需要发送完整的响应。

**实体报头**(Entity headers): 描述消息体内容。如`Content-Length`，`Content-Language`，`Content-Encoding`.

**2. content-type** 用于指示资源的 MIME 类型 [media type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

`text/plain, text/html, text/javascript, application/json, image/jpeg, multipart/form-data` ...

**3. tcp 协议** 三次握手 https://zhuanlan.zhihu.com/p/58603455

![](https://user-gold-cdn.xitu.io/2020/7/4/1731a1aad67c42a1?w=720&h=409&f=jpeg&s=32374)

**4. node.js 多个进程监听同一个端口** 没找到..

**5. node.js 模块**查找机制 顺序
[参考](https://juejin.im/post/5e02316a6fb9a016280140b1#heading-5)

- 核心模块(内置模块): 在 Node 进程开始的时候就预加载了
- 自定义模块: npm 上的第三方模块或自己写的 js 模块

给定的路径可以是省去`.js`后缀的文件名, 如果是一个文件夹, 在文件夹没有`package.json`指定入口的情况下导入`index.js`. 如果给定的不是路径而是第三方包名, 则会先在当前目录下的`node_modules`下查找, 如果没找到则会到上一级目录下的`node_modules`里找, 一直到根目录.

导入有缓存, 导入时会先判断是否已经存在于内存缓存中, 存在就不会再导入.

其他: 导入时若参数没有文件扩展名，会按`.js、.json、.node`的顺寻补足扩展名，依次尝试。

**6. koa** 有哪些抽象对象 application 中间件机制 - 洋葱模型

- context (ctx) 对象: 表示一次对话的上下文(包括`request`和`response`)。通过加工这个对象，就可以控制返回给用户的内容。[docs](https://www.npmjs.com/package/koa#context-request-and-response)
- 中间件(middleware): 同步/异步中间件 调用`next()`交给下一个中间件. [docs](https://www.npmjs.com/package/koa#middleware)

### part2

**1. html 的缓存如何设置**
http 缓存主要在响应头中设定:

- `Cache-Control`: 通过指定指令来实现缓存机制。详细参考[链接](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control) ✔
- `Expires`: 响应头包含日期/时间， 即在此时候之后，响应过期。如果在 Cache-Control 响应头设置了 "max-age" 或者 "s-max-age" 指令，那么 Expires 头会被忽略。
- `Etag`: 缓存未更改的资源。如果用户再次访问给定的 URL（设有 ETag 字段），显示资源过期了且不可用，客户端就发送值为 ETag 的 `If-None-Match` 的 header 字段. 服务器将客户端的 ETag（作为 If-None-Match 字段的值一起发送）与其当前版本的资源的 ETag 进行比较，如果两个值匹配（即资源未更改），服务器将返回不带任何内容的 304 未修改状态，告诉客户端缓存版本可用（新鲜）。
- `Last-Modified`: 响应首部，其中包含源头服务器认定的资源做出修改的日期及时间。 它通常被用作一个验证器来判断接收到的或者存储的资源是否彼此一致。由于精确度比 ETag 要低，所以这是一个备用机制。包含有 If-Modified-Since 或 If-Unmodified-Since 首部的条件请求会使用这个字段。

Etag 如何生成的? [参考](https://www.cnblogs.com/xianwang/p/12019830.html)

**2. http 强缓存、协商缓存** [参考](https://www.cnblogs.com/wonyun/p/5524617.html)✔

![](https://user-gold-cdn.xitu.io/2020/7/9/17332a3185892c7d?w=554&h=528&f=png&s=42440)

|          | 获取资源形式 | 状态码                        | 是否发送请求到服务器             |
| -------- | ------------ | ----------------------------- | -------------------------------- |
| 强缓存   | 从缓存取     | 200（from memory/disk cache） | 否，直接从缓存获取               |
| 协商缓存 | 从缓存取     | 304（not modified）           | 是，通过服务器来判断缓存是否可用 |

如果是 200 OK ，响应会带有头部`Cache-Control, Content-Location, Date, ETag, Expires` 和 `Vary`. [MDN: 304 Not Modified](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/304)

### part3

**1. js 数据类型，如何区分** [MDN:数据结构和类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%92%8C%E7%B1%BB%E5%9E%8B)

最新的 ECMAScript 标准定义了 8 种数据类型：

```
七种基本数据类型:
- 布尔值（Boolean）: 有2个值分别是：true 和 false.
- null: 一个表明 null 值的特殊关键字。(JavaScript是大小写敏感的)
- undefined: 和 null 一样是一个特殊的关键字，undefined 表示变量未定义时的属性。
- 数字（Number）: 整数或浮点数，例如：42 或者 3.14159。
- 大整数 (BigInt): 可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。
- 字符串（String）: 字符串是一串表示文本值的字符序列。
- 符号（Symbol）: (在 ECMAScript 6 中新添加的类型)。一种实例是唯一且不可改变的数据类型。
对象（Object）:对象是最复杂的数据类型，又可以分成三个子类型。
- 狭义的对象（object）{}
- 数组（array）[]
- 函数（function）
- Date RegExp 不是JavaScript数据类型。
```

通常`数值、字符串、布尔值`这三种类型，合称为原始类型（primitive type）的值，即它们是最基本的数据类型，不能再细分了。对象则称为合成类型（complex type）的值，因为一个对象往往是多个原始类型的值的合成，可以看作是一个存放各种值的容器。至于`undefined`和`null`，一般将它们看成两个特殊值。[参考](https://wangdoc.com/javascript/types/general.html#%E7%AE%80%E4%BB%8B).

---

JavaScript 有三种方法，可以确定一个值到底是什么类型。

- `typeof`运算符
- `instanceof`运算符
- `Object.prototype.toString.call`方法, 参数为要判断的值

```js
typeof 123; // "number"
typeof "123"; // "string"
typeof false; // "boolean"
typeof function () {}; // "function"
typeof undefined; // "undefined"
typeof {}; // "object"
typeof []; // "object" 在JavaScript内部，数组本质上只是一种特殊的对象
typeof null; // "object"
```

**2. instanceof 如何实现的** [MDN instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)

`instanceof` 运算符用于检测构造函数的 `prototype` (`constructor.prototype`) 属性是否出现在给予对象实例的原型链上。语法: 前接要判断的对象实例, 后接构造函数.

```js
const obj = {},
  arr = [];
obj instanceof Array; // false
obj instanceof Object; // true
arr instanceof Array; // true
arr instanceof Object; // true 因为 Object.prototype.isPrototypeOf(arr) 返回 true

Object.prototype.toString.call(arr); // "[object Array]"
Object.getPrototypeOf(arr) === Array.prototype; // true
arr.__proto__ === Array.prototype; // true
```

有一些坑: [StackOverflow](https://stackoverflow.com/questions/203739/why-does-instanceof-return-false-for-some-literals)

```js
var color1 = new String("green");
color1 instanceof String; // returns true
var color2 = "coral";
color2 instanceof String; // returns false (color2 is not a String object)
```

**3. 原型链** MDN: [对象原型](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Object_prototypes), [JavaScript 中的继承](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Objects/Inheritance), [继承与原型链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) 🏫

**4. new** [参考: 构造函数](https://juejin.im/post/5de4fe1d5188255e8b76e1f2#heading-4)

```js
function Fn() {}
const f = new Fn();
f.__proto__ === Fn.prototype; // true
```

```js
var a = new Foo("zhang","jake");
new Foo{
    var obj = {};
    obj.__proto__ = Foo.prototype;
    var result = Foo.call(obj,"zhang","jake");
    return typeof result === 'obj'? result : obj;
}
```

若执行 new Foo()，过程如下：

1. 创建新对象 obj；
2. 给新对象的内部属性赋值，构造原型链（将新对象的隐式原型指向其构造函数的显示原型）；
3. 执行函数 Foo，执行过程中内部 this 指向新创建的对象 obj（这里使用了 call 改变 this 指向）；
4. 如果 Foo 内部显式返回对象类型数据，则返回该数据；否则返回新创建的对象 obj。

[箭头函数不可使用 new](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/101) 🏫 , new 的过程大致是:

```js
function newFunc(father, ...rest) {
  var result = {};
  result.__proto__ = father.prototype;
  var result2 = father.apply(result, rest);
  if (
    (typeof result2 === "object" || typeof result2 === "function") &&
    result2 !== null // 因为 typeof null === 'object', 历史遗留问题
  ) {
    return result2;
  }
  return result;
}
```

### part4

```js
for (var i = 0; i < 5; i++) {
  setTimeout(function () {
    console.log(new Date(), i);
  }, 1000);
}
// 有几次打印，值是什么？
```

答: 五次 每次都是 '{时间} 5', 时间还是一样的.

原因: 五次循环十分快速的设定了 5 个 timeout, 等到这 1000ms 过去后 i 的值已经为 5, 而因为循环执行非常快, 他们打印的时间也相隔非常小, 故打印时间是一样的.

### part5

**1. Promise** 打印先后顺序

```js
console.log("script start"); // 1
let promise1 = new Promise(function (resolve) {
  console.log("promise1"); // 2
  resolve();
  console.log("promise1 end"); // 3
}).then(function () {
  console.log("promise2"); // 5
});
setTimeout(function () {
  console.log("settimeout"); // 6
});
console.log("script end"); // 4
```

JS 引擎线程是同步的单线程, 它维护一个执行栈.

原因: 构造 `Promise` 时会立即执行回调函数, `.then(fn)` 不会立即执行回调函数, 而是放入任务队列中, 待 JS 引擎线程的执行栈为空时, 事件触发线程才会从消息队列取出一个任务（即异步的回调函数）放入执行栈中执行.

而 `setTimeout` 虽然没给定时间(或给 0ms), 它都是由定时触发器线程执行, 时间到后放入消息队列中, 待执行栈空闲时才会被取出调用. 虽然代码的本意是 0 毫秒后就推入事件队列，但是 W3C 在 HTML 标准中规定，规定要求`setTimeout`中低于 4ms 的时间间隔算为 4ms。

而最后一行打印是在执行栈中的立即执行, 执行完后才空闲.

```js
console.log("script start"); // 1
setTimeout(function () {
  console.log("timer over"); // 5
}, 0);
Promise.resolve()
  .then(function () {
    console.log("promise1"); // 3
  })
  .then(function () {
    console.log("promise2"); // 4
  });
console.log("script end"); // 2
```

所有任务分为 `macrotask` 和 `microtask`:

- `macrotask`/`tasks`： 主代码块、setTimeout、setInterval 等
- `microtask`/`jobs`： Promise、process.nextTick 等

在执行宏任务时遇到`Promise`等，会创建微任务（`.then()`里面的回调），并加入到微任务队列队尾。在某一个`macrotask`执行完后，在重新渲染与开始下一个宏任务之前，就会将在它执行期间产生的所有`microtask`都执行完毕 (在渲染前). [参考](https://juejin.im/post/5be5a0b96fb9a049d518febc#heading-4) [图片参考](https://juejin.im/post/5a6547d0f265da3e283a1df7#heading-21)
![](https://user-gold-cdn.xitu.io/2020/7/8/1732daa0c38626ca?w=392&h=740&f=png&s=61349)

**2. 事件循环** [参考 1](https://juejin.im/post/5be5a0b96fb9a049d518febc) [参考 2](https://juejin.im/post/5a6547d0f265da3e283a1df7#heading-17)

执行栈, 消息队列/任务队列, 事件触发线程, 定时触发器线程...

渲染进程的线程: JS 引擎线程, 事件触发线程, 定时触发器线程...

![](https://user-gold-cdn.xitu.io/2020/7/8/1732dae325799234?w=610&h=637&f=png&s=68501)

**3. Promise 中的状态** pending, fulfilled, rejected.

**4. P.then().then() 如何接上去的**
因为 `Promise.prototype.then` 和 `Promise.prototype.catch` 方法返回`Promise`对象， 所以它们可以被链式调用。[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

![](https://user-gold-cdn.xitu.io/2020/7/4/17317bc264a5e618?w=801&h=297&f=png&s=26265)

### part6

**链表**节点构造函数如下：

```js
function ListNode(val) {
  this.val = val;
  this.next = null;
}
// 输入: 1->2->3->NULL
// 输出: 3->2->1->NULL
```

实现单链表反转 `function reverseList(head) {}` 返回反转后的链表头节点

```js
function ListNode(val) {
  this.val = val;
  this.next = null;
}

const obj = {};
const list = ["a", "b", "c"];

list.forEach((item, index) => {
  obj[item] = new ListNode(index + 1);
});

list.forEach((item, index) => {
  obj[item].next = obj[list[index + 1]] || null;
});

/**
 * @param {ListNode} head
 * @return {ListNode}
 */
const reverseList = function (head) {
  let pre = null;
  while (head) {
    next = head.next; // 当前节点的next链
    head.next = pre; // 当前节点下一个指向pre链
    pre = head; // 重新指向新链的头
    head = next; // 指回旧链
  }
  return pre;
};

reverseList(obj.a);
console.log(obj);
```

### part7

**1. 实现函数**

```js
function sum() {}
console.log(sum(2, 3)); // 输出5
console.log(sum(2)(3)); // 输出5
```

实现中只考虑题中两种传参方式.

```js
function sum() {
  if (arguments[1]) {
    return arguments[0] + arguments[1];
  }
  const first = arguments[0];
  return function tmp(add) {
    return first + add;
  };
}
console.log(sum(2, 3)); // 5
console.log(sum(2)(3)); // 5
```

**2. 箭头函数与 function 的区别, this 指向** 🏫

- [this、apply、call、bind - 掘金](https://juejin.im/post/59bfe84351882531b730bac2)
- [ES6 - 箭头函数、箭头函数与普通函数的区别 - 掘金](https://juejin.im/post/5c979300e51d456f49110bf0#heading-1)
- [「前端料包」一文彻底搞懂 JavaScript 中的 this、call、apply 和 bind - 掘金](https://juejin.im/post/5de4fe1d5188255e8b76e1f2)

```
1、箭头函数的 this 始终指向函数定义时的 this. (this 永远指向最后调用它的那个对象)
  箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，
  如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this
2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。
4、不可以使用 new 命令，因为：
  没有自己的 this，无法调用 call，apply。
  没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__
```

**3. webpack 中, `loader`和`plugin`的执行时机**

**4. vue 双向数据绑定** MVVM 🔨
