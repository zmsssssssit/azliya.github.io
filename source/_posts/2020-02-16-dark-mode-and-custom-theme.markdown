---
layout: post
title: '我的第一个POSTCSS plugin'
date: 2020-02-16 12:47:24 +0800
tags:
  - 前端开发
  - POSTCSS
---

我写了一个 POSTCSS 插件 [postcss-color-summary]() 帮助整理 CSS 颜色数据。

# 面临的问题与动机

工作上要对已有的 WebApp 项目做 OEM 和皮肤功能支持。我查阅了目前一些流行框架的解决方案：

| 方案来源    | 原理                                                    | 优点                     | 缺点                                         |
| ----------- | ------------------------------------------------------- | ------------------------ | -------------------------------------------- |
| ant-design  | less 框架，使用 less 变量控制 theme                     | 使用 less 变量           | 现有 webapp 用的是 postcss，迁移工作量大     |
| material-ui | 使用 js 对`<style>`标签进行替换，部分使用 css-in-js     | 直接用 js 管理           | 维护 js 映射，维护工作量大，而且迁移工作量大 |
| gitlab      | 利用 css 选择器，控制 root 的 classname，并写多套 style | 原理级方案，没有学习成本 | 维护成本高（N 皮肤数 \* M 选择器数 ）        |

基本是这三件套的组合方案：

1. CSS 变量
2. css-in-js
3. CSS 选择器+多套 style

但是，直接换成以上方案是，时间和工作量上都是不现实的。因为几乎所有方案，对于我手头上的 POSTCSS “老”项目来说，迁移成本都很大：

1. CSS 变量：整理统一 CSS 颜色，大规模重构
2. css-in-js：整理统一 CSS 颜色 + 换框架，大规模重构，日后维护“类 material-ui”的颜色样式映射工作量大
3. CSS 选择器+多套 style：整理统一 CSS 颜色，大规模重构，日后维护工作量=N\*M

# 方案

但是无论使用哪种方案，都需要将整个 WebApp 的颜色系统整理出来，才能够展开下一步。

手动去整理是不现实的：

1. 产品 CSS 量非常大；
2. 产品使用了 POSTCSS 的 color-function plugin，颜色会是一个运算结果；
3. 产品使用的 CSS color 变量多种多样，难以对比颜色的相似度，手动整理 platte 非常费力不讨好。

需要对预处理过的 CSS 进行分析整理， POSTCSS 的 AST 正好能帮上忙，所以我挖了个坑去帮我们自动去做这些事情：

1. 将颜色整理输出成 CSS 变量；
2. 整合阈值内相似颜色；
3. 支持对 postcss-color-function 处理的颜色分析

# POSTCSS plugin 速成

# POSTCSS AST

目前小插件功能比较简单，我要做的就是操作 AST 和使用简单的 NodeJs 就足够了。

POSTCSS 提供了一个 [AST explorer](https://astexplorer.net/#/2uBU1BLuJ1) 预览 AST 的数据结构
