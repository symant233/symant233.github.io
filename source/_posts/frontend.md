---
title: frontend
date: 2021-11-15 22:36:24
tags:
categories:
---

# 前端

- 前端通用技术文档 MDN https://developer.mozilla.org/zh-CN/
- **墙裂建议**IDE 用 [Visual Studio Code](https://code.visualstudio.com/)

### 基础

- 网络：《计算机网络》TCP/IP，DNS，域名。HTTP[概述](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview), [响应码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status), [请求响应头](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)...

- HTML：[w3](https://www.w3schools.com/html/html_intro.asp) 或 [runoob](https://www.runoob.com/html/html-tutorial.html)。重点 表单。

- CSS：[w3](https://www.w3schools.com/css/css_intro.asp) or [runoob](https://www.runoob.com/css/css-tutorial.html)。重点 ~~浮动布局~~，grid 布局，flex 布局；display, position; 盒模型。

HTML，CSS 过一遍，常用的记住就行了，其他的随时查。还是得多写。

- **JavaScript**：w3 或者直接看[ES6](https://es6.ruanyifeng.com/#docs/intro)。重点基础，学习语法，DOM 操作，Fetch API/XMLHttpRequest(XHR)，

- Node.js：[《Node.js 实战（第二版）》](https://www.ituring.com.cn/book/1993), npm/yarn 包管理器, js 的服务器运行环境，现在搭前端项目都需要用到。
<!-- more -->
- Git：版本控制。必须会基础的操作，[ref](https://juejin.cn/post/6844904115533774856),

- 杂项：json, jsonwebtoken, base64, urlencode,

### 框架

- JS 框架：**Vue.js**, **React.js**+jsx+typescript {hard}. 建议从 Vue 入手。

[Vue3 文档](https://v3.cn.vuejs.org/guide/introduction.html)好好看，直接 v3 省的后面还迁移。搭配[vue-router](https://router.vuejs.org/zh/guide/), 如果要多组件持续存储再加 vuex. 先学 HTML+CSS+JS 基础，然后可以考虑自己想个点子/搜视频，用 Vue 搭一个项目，边写项目边学。

- 样式框架：Bootstrap, **AntDesign**, Bulma... 有很多，省事但不要过于依赖，关键问题还是得自己写 css 代码解决。[Vuetify](https://vuetifyjs.com/zh-Hans/getting-started/installation/),

### 进阶

- 网络：RSA 非对称加密原理，HTTPS/SSL，WebSocket，

- JavaScript：[EcmaScript6](https://es6.ruanyifeng.com/#docs/intro), babel, 桌面应用 Electron, 构建工具 webpack, 移动端 React Native。

- TypeScript: js 的超集，严格的类型定义... [official doc](https://www.typescriptlang.org/docs/handbook/intro.html) {hard}

- CSS：类名命名规范[NEC](http://nec.netease.com/standard/css-sort.html). 预处理器 Sass/Scss, Less, stylus; [《深入解析 CSS》](https://www.ituring.com.cn/book/2583), 响应式.

### 安全

- 应用安全：[CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS), 鉴权[Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization), [HSTS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Strict-Transport-Security)，

- 攻击防御：[CSRF](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html), XSS, PWN, SQL 注入（这个要求不高，出问题后端背锅），文件上传漏洞（现在前后端分离就没这种了，除非 php asp jsp），...
