---
layout: post
title: 'Electron学习笔记'
date: 2020-06-07 12:47:24 +0800
tags:
  - Nodejs
  - Electron
---

# 进程

有两种进程：main和render

全局有唯一的main进程，由它来管理所有render进程。

main进程运行在：Node.js + Electron 环境中

render进程运行在：Node.js + Electron + Chromium 环境中

也就是说每个render进程都有自己的window、document全局对象，可以理解为每个由`BrowserWindow`构造的页面，都有自己的render进程

## main进程入口

一般来说，main进程在package.json的main属性中配置的文件即main进程（也可以通过 `electron xxx/myMainIndex.js`指定main进程入口）

## 一个简单的例子

在main进程中，通过`electron.BrowserWindow`类来管理render进程。

```js
const { app, BrowserWindow } = require('electron')
let win = null

app.on('ready', () => {
  win = new BrowserWindow({ width: 800, height: 600 })
  win.loadURL('https://github.com')
})
```



## 进程间通信

跨进程触发某种行为、跨进程通信

### main => render

#### ipcMain

https://www.electronjs.org/docs/api/ipc-main

 `ipcMain` 是 [Event Emitter](https://nodejs.org/api/events.html#events_class_eventemitter)的实例。 当在main进程中使用时，它处理从render进程（网页）发送出来的异步和同步信息。 从render进程发送的消息将被发送到该模块。

### render => main

要完成render到main进程的通信，你可以在render进程中使用以下对象

#### `electron.remote`

> `electron.remote`: 在渲染进程中使用主进程模块。RPC方式通信。

**注意事项：** 

* 使用时应注意不要泄露render对象
* 因为安全原因，remote 模块能在以下几种情况下被禁用：
  * [`BrowserWindow`](https://www.electronjs.org/docs/api/browser-window) - 通过设置 `enableRemoteModule` 选项为 `false`
  * [`<webview>`](https://www.electronjs.org/docs/api/webview-tag)- 通过把 ` enableremotemodule`属性设置成 `false`
* 每次构造remote对象或使用remote对象的方法，实际是在发送一个同步进程间状态的消息
* 跨进程访问到remote中的Array或Buffer类型属性，都是一份“深拷贝”，所以直接修改属性，效果不会跨进程
* 使用remote，会在render进程中保留一份引用（main进程不再会销毁这个引用用到的对象），要注意销毁，否则会影响gc
* 可以将render进程中的方法通过export或事件回调的方式暴露被main进程使用，但是要注意：
  * 在其他进程中export的方法，通过remote拿到会被**异步调用**
  * 添加事件监听，将回调挂在remote对象上，要注意手动卸载监听，否则会被重复添加（如果使用箭头函数，则更会导致内存泄漏）

如：从render进程总创建浏览器创建浏览器窗口（即，让render进程对象**能够使用main进程对象的方法**）

```js
// 在某个render进程对象中
const { BrowserWindow } = require('electron').remote
let win = new BrowserWindow({ width: 800, height: 600 })
win.loadURL('https://github.com')
```



#### ipcRenderer

`ipcRenderer` 是一个 [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter) 的实例。 你可以使用它提供的一些方法从render进程 (web 页面) 发送同步或异步的消息到main进程。 也可以接收main进程回复的消息

https://www.electronjs.org/docs/api/ipc-renderer



### render_A <==> render_B

#### HTML5 API

在两个网页（渲染进程）间共享数据最简单的方法是使用浏览器中已经实现的 HTML5 API，如 [Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Storage)， [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)，[`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) 或者 [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)

#### electron.remote

将数据放到main进程中使用remote模块访问