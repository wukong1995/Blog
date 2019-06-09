---
title: 停止使用css in js进行web开发
date: 2019-06-09 16:35:51
tags: [前端, css]
categories: [前端]
description: Stop using CSS in JavaScript for web development
---

CSS不会去任何地方。许多项目选择用JavaScript来设置文档样式是出于错误的原因。本文列出了常见的误解（神话）和现有的CSS解决方案。

>本文中的任何内容均不构成对特定项目或个人的人身攻击

### CSS和JavaScript的历史
层叠样式表（CSS）语言用于描述用标记语言编写的文档的表示。JavaScript作为“粘合语言”被创建，它用于组合图像和插件等组件。多年来，JavaScript不断发展并变异以适应新的用例。

Ajax（2005年）的出现标志着一个重要的里程碑。 这就是Prototype，jQuery，MooTools等库吸引大量社区共同努力解决后台和跨浏览器不一致的数据获取问题。 这造成了一个新问题：如何管理所有数据？

快进到2010年，Backbone.js作为应用程序状态管理的行业标准。不久之后，Knockout和Angular将每个人都用双向绑定来吸引人。不久之后，React和Flux出现了。这开启了单页应用程序（SPA）、由组件组成的应用程序的时代。

### 那么CSS呢？
借用`styled-components`的文档：
>The problem with plain CSS is that it was built in an era where the web consisted of documents. In 1993 the web was created to exchange mostly scientific documents, and CSS was introduced as a solution to style those documents. Nowadays however, we are building rich, interactive, user-facing applications, and CSS just isn’t built for this use-case.

我不同意。

CSS已经发展到捕获现代UI的要求。过去十年中新功能的数量超出了合理范围（pseudo-classes, pseudo-elements, CSS variables, media queries, keyframes, combinators, columns, flex, grid, computed values, …）

从UI的角度来看，“组件”是文档的孤立片段（`<button />`是组件）。CSS旨在设计文档样式，其中包含所有组件。有什么问题？

俗话说：“使用合适的工具来完成工作”。

### Styled-components
`Styled-components`使用标记模板文字在JavaScript中编写CSS。它删除了组件和样式之间的映射 - 组件被制成一个低级样式构造，例如，

```js
import React from 'react';
import styled from 'styled-components';
// Create a <Title> react component that renders an <h1> which is
// centered, palevioletred and sized at 1.5em
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;
// Create a <Wrapper> react component that renders a <section> with
// some padding and a papayawhip background
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;
// Use them like any other React component – except they're styled!
<Wrapper>
  <Title>Hello World, this is my first styled component!</Title>
</Wrapper>
```

结果是：
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1560070444/blog/jjj.png)

styled-components现在成为在React中设置组件样式的新方法。
>让我们明白一点：样式组件只是CSS之上的高级抽象。它所做的就是解析用JavaScript定义的CSS并创建映射到CSS的JSX元素。

我不喜欢这种趋势，因为它被许多误解所包围。

下面是选择使用styled-components的原因列表，我结合原文谈一下我的观点。

#### 1.它解决了全局命名空间和样式冲突
听起来它似乎没有解决这些问题。 CSS模块，Shadow DOM和无数的命名约定（eg: BEM）很久以前就在社区中解决了这个问题。

styled-components（就像CSS模块一样）消除了人类命名。但就其本身而言，这并不是开始使用styled-components的充分理由。

#### 2.使用styled-components可以获得更简洁的代码
举一个例子
```js
<TicketName></TicketName>
<div className={styles.ticketName}></div>
```
首先 - 没关系。差异可以忽略不计。
其次，事实并非如此。总字符数取决于样式名称。
```js
<TinyBitLongerStyleName></TinyBitLongerStyleName>
<div className={styles.longerStyleName}></div>
```
>这样的做法和css中的选择器有什么差异吗

#### 3. 使用styled-components可以让您更多地考虑语义
语义和样式是两个不同的东西。

#### 4.它可以轻松扩展样式
css预处理器不是可以很轻松的解决这个问题

#### 5.它使条件样式组件变得容易
通过变量更改className不是一个好的解决办法吗？

#### 6.它允许更好的代码组织
这个完全不同意。都成一锅粥了...本来一个一百行的组件要是和样式写在一起，都要二百行了...

#### 7.任何更好的性能，任何更小的捆绑尺寸
js做了解析，在不同的页面挂载不同的css，小，我是同意的，但是对于一个spa来说，你的css没有办法进行缓存。

另外通过查看源码可以看到，css实际上都是挂载在一个HOC上面，这很明显也存在一定的性能问题。

如果使用ssr的话，你得到的可能是一个巨大的HTML。

#### 8.它允许开发响应组件
🙂️

### 结语
我现在知道很多公司正在使用css in js，我想大部分的因素应该是第一个css和组件的映射。换一种角度来说，我认同组件的定义，但是对于它和css的关系，我会认为是html和css的关系。如果要在css上做区分的话，我会选择给组件的className加一个rc的前缀。

对于命名，有很多成熟的方案，例如之前使用的BEM，其实我在css in js的代码里面看到最多的是嵌套语法，这个就很伤了，尽管处理器现在已经很快了，那也禁不住你三四层的套啊。

css是样式，js对于一个平台来说已经很巨大了，将css给它处理真的是一个累赘。还有一点开发过程，写css和写css in js完全是存在巨大差异的过程，没有各种简写，我只能选择浏览器作为我的第一开发工具😂。

[参考链接](https://medium.com/@gajus/stop-using-css-in-javascript-for-web-development-fa32fb873dcc)







