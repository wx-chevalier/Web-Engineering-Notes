# Web 构建与打包工具

# 构建工具的发展历史

自 2011 年开始随着前端项目复杂度的增加，社区提出了很多工具或者框架的方案；首先崛起的是以 RequireJS 与 SeaJS 为代表的模块加载器(Module Loader)；虽然譬如 Yui、Dojo 这些当时流行的前端框架已经有了自己的模块化支持方案，但是 RequireJS 是首个通用的流行前端模块化规范。不过 RequireJS 中所有模块都是异步加载，导致了发布到生产环境时会非常麻烦，因此又出现了如 r.js 这样的能够帮助模块加载器进行文件合并与压缩的工具。

此时在开发流程中需要针对每个入口文件使用 r.js 等工具进行打包，于是社区又提出了以 Grunt、Gulp 为代表的任务自动化运行工具(Task Runner)。Grunt 与 Gulp 殊途同归，前者基于临时文件进行构建，后者通过文件流处理；但是它们都是旨在解决文件自动化处理问题，通过结合模块加载器、加载器打包工具以及任务自动化工具，我们可以实现初步的开发流程自动化。虽然任务自动化运行工具允许开发者在项目中配置一些自动化的任务以便捷进行文件合并、代码压缩、后处理等操作，不过它们存在的问题在于其不能够去真正的自动化解析依赖，并且对于 HTML、CSS、JavaScript 这些不同类型的资源文件只能分割独立地处理，无形会大大拉低开发部署的速度。

## 第三纪元

JS 开发工具从去年开始又出现新一轮更新换代的征兆，有人把这种趋势称作 JS 的第三纪元，新范式的项目涌现，开始进入到日常业务的开发实践，很多场景下已经没有 Webpack。

![构建工具的第二纪元与第三纪元](https://assets.ng-tech.icu/superbed/2021/07/06/60e3ee325132923bf8042eb1.jpg)

图中右侧的 esbuild 和 swc 这样的构建工具，把编译、构建、打包、压缩等在 Webpack 里属于不同环节的部分，合并在一起，加上非 JS 的系统编程语言的帮助，大幅提升构建速度。另一方面，也能支持 ESM 优先的、不需要打包的构建场景。Snowpack、Vite 这样的工具，在此基础上实现了开发者体验（DX）优先的、不打包（Unbundled）的开发调试模式。

这些工具和模式跟 Webpack 的设计有一些本质矛盾，目前已经被用于公共库的构建、业务项目的开发调试等真实场景里。

# 构建工具的问题

## 小模块

另一个笔者想提到的所谓小模块问题，模块打包工具能够有效地帮我们自动处理模块之间的依赖关系，不过因为现在我们在进行模块打包的同时会进行大量的转换或者 Polyfill 的工作，导致了模块过多时最终的生成包体中会包含大量的胶水代码。譬如我们编写了两个简单的模块仅仅会导出一些常量：

```js
// index.js
var total = 0;
total += require("./module_0");
total += require("./module_1");
total += require("./module_2");
// etc.
console.log(total);
// module_0.js
module.exports = 0;
// module_1.js
module.exports = 1;
```

我们如果使用 Browserify 或者 Webpack 进行打包，其会将每个模块包裹进独立的函数作用域中，然后声明一个顶层的运行时加载器，譬如上述代码在 Browserify 中的打包结果如下：

```js
(function e(t,n,r){function s(o,u){if(!n[o]){if(!t[o]){var a=typeof require=="function"&&require;if(!u&&a)return a(o,!0);if(i)return i(o,!0);var f=new Error("Cannot find module '"+o+"'");throw f.code="MODULE_NOT_FOUND",f}var l=n[o]={exports:{}};t[o][0].call(l.exports,function(e){var n=t[o][1][e];return s(n?n:e)},l,l.exports,e,t,n,r)}return n[o].exports}var i=typeof require=="function"&&require;for(var o=0;o<r.length;o++)s(r[o]);return s})({1:[function(require,module,exports){
module.exports = 0
},{}],2:[function(require,module,exports){
module.exports = 1
},{}],3:[function(require,module,exports){
module.exports = 10
},{}],4:[function(require,module,exports){
module.exports = 100
// etc.
```

而 Rollup 或者 Closure 打包的结果看起来会舒适很多：

```js
(function () {
'use strict';
var module_0 = 0
var module_1 = 1
// ...
total += module_0
total += module_1
// etc.
```

不过在大量模块的情况下，任何一种打包工具包体的增长速度会随着模块数的增长而变大：

| Bundler              | 100 modules | 1000 modules | 5000 modules |
| -------------------- | ----------- | ------------ | ------------ |
| browserify           | 7982        | 79987        | 419985       |
| browserify-collapsed | 5786        | 57991        | 309982       |
| webpack              | 3955        | 39057        | 203054       |
| rollup               | 1265        | 13865        | 81851        |
| closure              | 758         | 7958         | 43955        |
| rjs                  | 29234       | 136338       | 628347       |
| rjs-almond           | 14509       | 121612       | 613622       |
