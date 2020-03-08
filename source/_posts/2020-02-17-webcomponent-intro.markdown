---
layout: post
title: 'WebComponent-0'
date: 2020-02-17 12:47:24 +0800
tags:
  - 前端开发
  - WebComponent
---

ref:[MDN: WebComponent](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

# WebComponent

ReactJS 和 Vue 等框架定义了自己的 DSL 去写 component，让开发者写可以复用的组件，优点是可以不用管各种 WebApi 的支持了，缺点是 component 就只能给这个框架使用。

原生的方案可以使用 WebApi 的这 3 件套，缺点是各厂商的支持情况不同，但是不受框架限制：

- **Custom elements**：一套 WebApi。用来自定义 HTML 标签。用来做你的 WC 的 api。
- **Shadow DOM**：一套 WebApi。用来渲染影子节点，放一些 WC 的脚本和样式，不会污染全局。
- **HTML templates**： [`<template>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) 和 [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) 标签。用来放 WC 的 HTML 模板，提高渲染效率和代码复用率。

写一个基本的 WC，一般有以下步骤：

1. 写一个 Class 或者 function（如 MyWebComponent）
2. 用[`CustomElementRegistry.define()`](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define) 注册刚刚声明的 MyWebComponent（如 `CustomElementRegistry.define("my-web-component", MyWebComponent, { extends: "div" })`）
3. [optional] 按需，你可以用 Shadow DOM，放一些局部的脚本和样式
4. [optional] 按需，你可以用[`<template>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) 和 [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot)写一些模板，然后在 WC 里复用起来
5. 然后你就可以在应用中，像使用`<div>`标签一样，使用`<MyWebComponent>`了，或者

> 可以参考一个[Google 的 Dark Mode 的 Web Component](https://googlechromelabs.github.io/dark-mode-toggle/demo/index.html)
>
> MDN 也提供了很多精彩的[Web Component 例子](https://github.com/mdn/web-components-examples)
