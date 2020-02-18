---
layout: post
title: 'WebComponent'
date: 2020-02-17 12:47:24 +0800
categories: blog, frontend, webcomponent,customcomponent
---

ref:[MDN: WebComponent](https://developer.mozilla.org/en-US/docs/Web/Web_Components)

# 什么是WebComponent

Web Component（下简称WC）是一种将细节封装成能够重复使用工具的技术。它们作为自定义元素，能够在你的WebApp中广泛使用。

本文主要聊一聊原生WebApi支持的，写一个自己的WC的工具有哪些。

使用WebApi写自定义WC，需要这3件套：

* **Custom elements**：一套WebApi。用来自定义HTML标签。用来做你的WC的api。
* **Shadow DOM**：一套WebApi。用来渲染影子节点，放一些WC的脚本和样式，不会污染全局。
* **HTML templates**： [`<template>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) 和 [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) 标签。用来放WC的HTML模板，提高渲染效率和代码复用率。

写一个基本的WC，一般有以下步骤：

1. 写一个 Class 或者 function（如 MyWebComponent）
2. 用[`CustomElementRegistry.define()`](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define) 注册刚刚声明的 MyWebComponent（如 `CustomElementRegistry.define("my-web-component", MyWebComponent, { extends: "div" })`）
3. [optional] 按需，你可以用Shadow DOM，放一些局部的脚本和样式
4. [optional] 按需，你可以用[`<template>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) 和 [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot)写一些模板，然后在WC里复用起来
5. 然后你就可以在应用中，像使用`<div>`标签一样，使用`<MyWebComponent>`了，或者

> 可以参考一个[Google的Dark Mode的Web Component](https://googlechromelabs.github.io/dark-mode-toggle/demo/index.html)
>
> MDN也提供了很多精彩的[Web Component例子](https://github.com/mdn/web-components-examples)

