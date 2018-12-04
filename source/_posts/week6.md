---
date: 2017-09-03 09:05:56
title: url上添加query
tags: [javascript]
categories: [前端, javascript]
description: url上添加query-跳转页面和无需跳转页面
---

举一个栗子🌰：在分页时，你通常会看到`url`上一般是`xxx?page=1`
通常的做法是点击第几页直接跳转页面，是通过location进行的
现在的需求是：在跳转页面的时候，我既想要改变url，同时我只需要改变分页的数据，而不需要整个页面重绘。
#### -----
很幸运`history`提供了这么一个方法`pushState`，它有三个参数：state object，title，以及一个可选的URL地址。第二个参数title:现在firefox和chrome已经忽略该参数
```js
window.history.pushState('','title','?page=1');
```
[pushState参考链接](https://developer.mozilla.org/en-US/docs/Web/API/History_API)

