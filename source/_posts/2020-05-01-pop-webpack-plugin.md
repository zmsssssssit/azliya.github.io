---
layout: post
title: '常用的Webpack plugin 和 loader'
date: 2020-05-01 12:47:24 +0800
tags:
  - Webpack
---

# loader

将文件转换成AST，加载进webpack核心

raw-loader

url-loader

filesize-loader

babel-loader

# plugin

帮助webpack将AST，处理、打包

ts-loader

babel-loader: 



## 常用plugin

html-webpack-plugin: 打包、处理html文件

script-ext-html-webpack-plugin: 自动将打包后的js加到HTML中

[`TerserPlugin`](https://webpack.js.org/plugins/terser-webpack-plugin/): wp4后被推荐用来[代替UglifyjsWebpackPlugin][1]

[DefinePlugin](https://webpack.js.org/plugins/define-plugin/): 在compile time定义全局常量

[CopyWebpackPlugin](https://webpack.js.org/plugins/copy-webpack-plugin/): 复制文件、目录





ref

[1]: https://github.com/webpack/webpack/issues/7923 “uglify-js已经停止维护了，社区推荐改用terser”

[2]:https://github.com/webpack-contrib/awesome-webpack "webpack推荐的loader和plugin"