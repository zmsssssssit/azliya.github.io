---
layout: post
title: '2020年更好的 Webpack 配置'
date: 2020-04-20 12:47:24 +0800
tags:
  - Webpack
---

https://medium.com/swlh/2020-settings-of-react-typescript-project-with-webpack-and-babel-403c92feaa06

使用webpack4、babel7以上

- 使用ts

- 使用webpack-merge，将config分块管理

- babel使用preset

- [使用 @babel/register][3]，将babel的next-generation-js的能力赋予nodejs原声的require hook，也就能够在所有场景中使用es6（原理是：sourceFile => webpack.interpret => babel-core.[babel-register][1]【All subsequent files required by node with the extensions `.es6`, `.es`, `.jsx` and `.js` will be transformed by Babel.】 => webpack的其他流程）

- [用@babel/preset-typescript代替ts-loader][2]: preset-typescript 是ts和babel团队合作1年多的产物，稳定可靠社区庞大，而且preset能够将ts=>js（babel会直接删除ts特有代码），绕过了ts的内核，变异速度更快，包依赖更少（编译时不再需要安装ts，只需要管理babel一个编译器）【但是这会绕过编译的ts类型检查，因此你需要手动配置好tsconfig.json并调用tsc命令（可以加上 --watch参数保持坚挺），触发ts检查】[阅读来源][4]

  



ref

[1]: https://babeljs.io/docs/en/next/babel-register.html "babel-register: The require hook will bind itself to node's require and automatically compile files on the fly"

[2]: https://www.cnblogs.com/vvjiang/p/12057811.html "使用@babel/preset-typescript取代awesome-typescript-loader和ts-loader"
[3]: https://medium.com/oredi/webpack-with-babel-7-b61f7caa9565#5a1e "Allow the Webpack configuration file to be written using ES6 is installed"
[4]:https://iamturns.com/typescript-babel/ "TypeScript With Babel: A Beautiful Marriage"

