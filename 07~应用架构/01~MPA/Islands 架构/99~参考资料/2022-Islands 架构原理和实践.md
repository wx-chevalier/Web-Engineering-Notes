Islands 架构是今年比较火的一个话题，目前社区一些比较知名的新框架如 Fresh、Astro 都是 Islands 架构的典型代表。本文将给大家介绍 Islands 架构诞生的来龙去脉，分析它相比于 Next.js、Gatsby 等传统方案的优势，并且剖析社区相关框架的实现原理，以及分享笔者在这个方向上的一些实践。

## MPA 和 SPA 的取舍

MPA 和 SPA 是构建前端页面常见的两种方式，理解 MPA 和 SPA 的区别和不同场景的取舍是理解 Islands 架构的关键。

### 概念

MPA(Multi-page application) 即多页应用，是从服务器加载多个 HTML 页面的应用程序。每个页面都彼此独立，有自己的 URL。当单击 a 标签链接导航到另一个页面时，浏览器将向服务器发送请求并加载新页面。例如，传统的模板技术如 JSP、Python、Django、PHP、Laravel 等都是基于 MPA 的框架，包括目前比较火的 Astro 也是采用的 MPA 方案。

SPA(Single-page application) 即单页应用，它只有一个不包含具体页面内容的 HTML，当浏览器拿到这份 HTML 之后，会请求页面所需的 JavaScript 代码，通过执行 JavaScript 代码完成 DOM 树的构建和 DOM 的事件绑定，从而让页面可以交互。如现在使用的大多数 Vue、React 中后台应用都是 SPA 应用。

### 对比

##### 1. 性能

在 MPA 中，服务器将响应完整的 HTML 页面给浏览器，但是 SPA 需要先请求客户端的 JS Bundle 然后执行 JS 以渲染页面。因此，MPA 中的页面的首屏加载性能比 SPA 更好。

但 SPA 在后续页面加载方面有更好的性能和体验。因为 SPA 在完成首屏加载之后，在访问其它的页面时只需要动态加载页面的一部分组件，而不是整个页面。而且，当页面发生跳转时，SPA 不会重新加载页面，对用户更友好。

##### 2. SEO

MPA 中服务端会针对每个页面返回完整的 HTML 内容，对 SEO 更加友好；而 SPA 的页面内容则需要执行 JS 才能拉取到，不利于 SEO。

##### 3. 路由

MPA 在浏览器侧其实不需要路由，每个页面都在服务端都有一份 URL 地址，浏览器拿到 URL 直接请求服务端即可。

但 SPA 则不同，它需要 JS 掌管后续所有路由跳转的逻辑，因此会引入一些路由方案来管理前端的路由，比如基于 hashchange 事件或者浏览器 history API 来实现。

##### 4. 状态管理

除了路由，SPA 另外一个复杂的点在于状态管理。SPA 当中所有路由的状态都是由 JS 进行管理，在不同的路由进行跳转时通过 JS 代码进行一些状态的流转，在页面的规模越来越大的时候，状态管理就变得越来越复杂了。因此，社区也诞生了不少的状态管理方案，如传统的 Redux、社区新秀 Valtio、Zustand 包括字节自研的 Reduck，都是为了解决 SPA 状态管理的问题，一方面降低操作的复杂度、另一方面引入一些规范和限制(比如 Redux 中的 action 机制)来提高项目可维护性。

而 MPA 则会简单很多，因为每个页面之间都是相互独立的，不需要在前端做复杂的状态管理。

### 取舍

总而言之，MPA 有更好的首屏性能，SPA 在后续页面的访问中有更好的性能和体验，但 SPA 也带来了更高的工程复杂度、略差的首屏性能和 SEO。这样就需要在不同的场景中做一些取舍。

不过，MPA 和 SPA 也并不是完全割裂的，两者也是能够有所结合的，比如 SSR/SSG 同构方案就是一个典型的体现，首先框架侧会在服务端生成完整的 HTML 内容，并且同时注入客户端所需要的 SPA 脚本。这样浏览器会拿到完整的 HTML 内容，然后执行客户端的脚本事件的绑定(这个过程也叫 `hydrate`)，后续路由的跳转由 JS 来掌管。当下很多的框架都是采用这样的方案，比如 Next.js、Gatsby、公司内部的 Eden SSR、Modern.js。

但实际上，把 MPA 和 SPA 结合的方案也并不是完美无缺的，主要的问题在于这类方案仍然会下载`全量的客户端 JS `及执行`全量的组件 Hydrate 过程`，造成页面的首屏 TTI 劣化。

我们可以试想对于一个文档类型的站点，其实里面的大多数组件是不需要交互的，主要以静态页面的渲染为主，因此直接采用 MPA 方案是一个比 `MPA + SPA` 更好的一个选择。进一步讲，对于更多的轻交互、重内容的应用场景，MPA 也依然是一个更好的方案。

由于页面中有时仍然不可避免的需要一些交互的逻辑，那放在 MPA 中如何来完成呢？这就是 Islands 架构所要解决的问题。

## 什么是 Islands 架构？

Islands 架构模型早在 2019 年就被提出来了，并在 2021 年被 Preact 作者`Json Miller` 在 Islnads Architecture 一文中得到推广。这个模型主要用于 SSR (也包括 SSG) 应用，我们知道，在传统的 SSR 应用中，服务端会给浏览器响应完整的 HTML 内容，并在 HTML 中注入一段完整的 JS 脚本用于完成事件的绑定，也就是完成 hydration (注水) 的过程。当注水的过程完成之后，页面也才能真正地能够进行交互。

当一个页面中只有部分的组件交互，那么对于这些可交互的组件，我们可以执行 hydration 过程，因为组件之间是互相独立的。

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IeGljcqktAvbbiba9e58TBomMNFc3ZmlW8lmDffibjqCtojIbxt1ibxj3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

而对于静态组件，即不可交互的组件，我们可以让其不参与 hydration 过程，直接复用服务端下发的 HTML 内容。

可交互的组件就犹如整个页面中的孤岛(Island)，因此这种模式叫做 Islands 架构。

## Islands 实现原理

### Astro

https://astro.build/

在 Astro 中，默认所有的组件都是静态组件，比如：

```
// index.astro
import MyReactComponent from '../components/MyReactComponent.jsx';
---
<MyReactComponent />
```

这种写法不会在浏览器添加任何的 JS 代码。但有时我们需要在组件中绑定一些交互事件，那么这时就需要`激活孤岛组件`了，在使用组件时加上`client:load`指令即可:

```
// index.astro
---
import MyReactComponent from '../components/MyReactComponent.jsx';
---
<MyReactComponent client:load />
```

Astro 除了支持本身 Astro 语法之外，也支持 Vue、React 等框架，可以通过插件的方式来导入。在构建的时候，Astro 只会打包并注入 Islands 组件的代码，并且在浏览器渲染，分别调用不同框架(Vue、React)的渲染函数完成各个 Islands 组件的 hydrate 过程。

> Astro 是典型的 MPA 方案，不支持引入 SPA 的路由和状态管理。

### Fresh

Fresh 是一个基于 Preact 和 Deno 的全栈框架，同时也主打 Islands 架构。它约定项目中的 `islands` 目录专门存放 island 组件:

```
.
├── README.md
├── components
│   └── Button.tsx
├── deno.json
├── dev.ts
├── fresh.gen.ts
├── import_map.json
├── islands                 // Islands 组件目录
│   └── Counter.tsx
├── main.ts
├── routes
│   ├── [name].tsx
│   ├── api
│   │   └── joke.ts
│   └── index.tsx
├── static
│   ├── favicon.ico
│   └── logo.svg
└── utils
    └── twind.ts
```

Fresh 在渲染层核心主要做了以下的事情:

- 通过扫描 islands 目录记录项目中声明的所有 Islands 组件。
- 拦截 Preact 中 vnode 的创建逻辑，目的是为了匹配之前记录的 Island 组件，如果能匹配上，则记录 Island 组件的 props 信息，并将组件用 的注释标签来包裹，id 值为 Island 的 id，数字为该 Island 的 props 在全局 props 列表中的位置，方便 hydrate 的时候能够找到对应组件的 props。
- 调用 Preact 的 renderToString 方法将组件渲染为 HTML 字符串。
- 向 HTML 中注入客户端 hydrate 的逻辑。
- 拼接完整的 HTML，返回给前端。

值得注意的是客户端 hydrate 方法的实现，传统的 SSR 一般都是直接对根节点调用 hydrate，而在 Islands 架构中，Fresh 对每个 Island 进行独立渲染。

> 更多细节可以参考篇文章：[深入解读 Fresh](https://mp.weixin.qq.com/s?__biz=MzU0MTU4OTU2MA==&mid=2247490454&idx=1&sn=c0d516b0a3c64cc3eefb8aca9f305ea0&scene=21#wechat_redirect)

## 实践分享

笔者基于 Islands 架构开发了一个文档站方案 island.js。

- 仓库: https://github.com/sanyuan0704/island.js

**大体定位是支持** **Mdx** **的类 VitePress 方案**，目前也实现了 Islands + MPA 架构，接下来给大家分享一下这个方案是如何来实现 Islands 架构的。

### 使用 Island 组件

与 Astro 类似，Island.js 里面默认采用 MPA 且 0 JS 的方案，如果存在存在交互的组件，在使用的时候传入一个`__island` 标志即可，比如:

```
import { Aside } from '../components/Aside';

export function Layout() {
  return <Aside __island />;
}
```

这样在生产环境打包的过程中自动识别出 Islands 组件并打包，在 hydrate 的时候各自执行 hydration。

### 内部实现

总体流程如下:

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IvtOUenSzkf0lfGqnEtNZyFLqge8dZ9nniaFtYT83NY1gB90XEibXgsYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

##### 1. SSR Runtime

指组件 renderToString 的过程，我们需要在这个运行时过程中搜集到所有的 Islands 组件。主要的实现思路是拦截组件创建的逻辑，在 React 中可以通过拦截 React.createElement 实现或者 jsx-runtime 来完成，Island.js 里面实现了后者，通过自定义 jsx-runtime 来拦截 SSR 运行时:

```
// island-jsx-runtime.js
import * as jsxRuntime from 'react/jsx-runtime';

export const data = {
  // 存放 islands 组件的 props
  islandProps: [],
  // 存放 islands 组件的文件路径
  islandToPathMap: {}
};

const originJsx = jsxRuntime.jsx;
const originJsxs = jsxRuntime.jsxs;

const internalJsx = (jsx, type, props, ...args) => {
  if (props && props.__island) {
    data.islandProps.push(props || {});
    const id = type.name;
    // __island 的 prop 将在 SSR 构建阶段转换为 `__island: 文件路径`
    data.islandToPathMap[id] = props.__island;
    delete props.__island;

    return jsx('div', {
      __island: `${id}:${data.islandProps.length - 1}`,
      children: jsx(type, props, ...args)
    });
  }
  return jsx(type, props, ...args);
};

export const jsx = (...args) => internalJsx(originJsx, ...args);

export const jsxs = (...args) => internalJsx(originJsxs, ...args);

export const Fragment = jsxRuntime.Fragment;
```

然后在 JSX 编译阶段，指定 jsxRuntime 参数为我们自定义的路径即可。

##### 2. Build Time

Build Time 分为两个阶段: renderToString 之前、renderToString 之后。

renderToString 之前会打两份 bundle:

- SSR bundle: 用于 renderToString
- Client bundle: 客户端 Runtime 代码，用于激活页面

在 SSR bundle 生成过程中，我们会特殊处理 `__island` prop，它实际上是为了标识该组件是一个 Islands 组件，但我们拿不到组件的路径信息。为了之后能够顺利打包 Islands 组件，我们需要在 SSR 构建过程中将 \_\_isalnd 进行转换，使之带上路径信息。比如下面有两个组件:

```
// Layout.tsx
import { Aside } from './Aside.tsx';

export function Layout() {
  return (
    <div>
      <Aside __island a={1} />
    </div>
  )
}

// Aside.tsx
export function Aside() {
  return <div>内容省略...</div>
}
```

可以看到 Layout 组件中通过`<Aside __island />` 的方式来使用 Aside 组件，标识其为一个 Islands 组件。那么我们将会在 SSR 编译过程中用 babel 插件改写这个 prop，原理如下:

```
<Aside __island />
// 被转换为
<Aside __island="./Aside.tsx!!island!!Users/project/src/Layout.tsx" />
```

这样，在 renderToString 过程中，我们就能记录下 Islands 组件所在的文件路径。当 renderToString 完成之后，我们可以通过自定义的 jsx-runtime 模块拿到如下的数据:

```
{
  islandProps: [ { a: 1 } ],
  islandToPathMap: {
    Aside: './Aside.tsx!!island!!Users/project/src/Layout.tsx'
  }
}
```

之后在 Build Time 会做两件事情:

1. 将 islandProps 的数据作为 id 为`island-props` 的 script 标签注入到 HTML 中；
2. 根据 islandToPathMap 的信息构造虚拟模块，打包所有的 Islands 组件。

虚拟模块内容如下:

```
import { Aside } from './Aside.tsx!!island!!Users/project/src/Layout.tsx';

window.islands = {
  Aside
};

window.ISLAND_PROPS = JSON.parse(
  document.getElementById('island-props').textContent
);
```

将这个虚拟模块打包后我们得到一份 Islands bundle，将这个 bundle 注入到 HTML 中以完成 Islands 组件的注册。

> 💡 问题: islands bundle 和 client bundle 有共同的依赖 React，由于在两次不同的打包流程中，所以 React 会打包两份。解决方案是 external 掉 react 和 react-dom 依赖，通过 import map 指向全局唯一的 React 实例。

##### 3. Client Runtime

在客户端渲染阶段，我们仅需要少量的脚本来激活 Islands 组件:

```
  import { hydrateRoot, createRoot } from 'react-dom/client';

  const islands = document.querySelectorAll('[__island]');
  for (let i = 0; i < islands.length; i++) {
    const island = islands[i];
    const [id, index] = island.getAttribute('__island')!.split(':');
    const Element = window.ISLANDS[id];
    hydrateRoot(
      island,
      <Element {...window.ISLAND_PROPS[index]}></Element>
    );
  }
```

由此，我们便在 React 实现了 Islands 架构，在实际的页面渲染过程中，浏览器仅需请求 React + 少量组件的代码甚至是 0 js。SSG + SPA 方案和 Islands 架构的页面加载情况对比如下:

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IiaoFR1zvpicacoWUticRvC0D2W3qw2zXibuMvlmvbTpXhfh8GfVd0UKKCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761Inib5rYYpy6F2Zm9KwK1uh3LmaTMdN5mw4YsCfJB5KHVpq3kkfedvTvA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IWhgdbwicnlBTQYppqlrlQSRundxiakkOx0y8ic8oMpSf2tlPpDm17Icag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)SSG + SPA

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)SSG + Islands architecture(MPA)

|              | HTTP 请求资源(KB) | FCP （s） | DCL(s)     |
| :----------- | :---------------- | :-------- | :--------- |
| SPA 模式     | 451               | 0.48      | 0.84       |
| Islands 模式 | 141               | 0.40      | 0.52       |
| 优化情况     | 减少近 60%        | 变化不大  | 提前近 40% |

### Islands 架构的适用性

##### 1. 框架无关

Island 架构的实现其实是可以做到框架无关的。从 SSR Runtime、Build Time 到 Client Runtime，整个环节中关于 React 的部分，我们都可以替换成其它框架的实现，这些部分包括:

- 创建组件的方法
- 组件转换为 HTML 字符串的 renderToString 方法
- 浏览器端的 hydrate 方法

因此，不光是 React，对于 Vue、Preact、Solidjs 这些框架中都可以实现 Islands 架构。因此，在 Island.js 中兼容除 React 的其它框架也是原理上完全可行的。

并且考虑到 React 的包体积问题，后续 Island.js 考虑适配其它的框架，如 Solid，体积相比 React 可以减少 90%:

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IBOvNpGZxIUWaQSZH3XNQlQpxdjyzaIicTrvgCia8jyH827F7Ls0bSQ6A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

> 数据来源: https://dev.to/this-is-learning/javascript-framework-todomvc-size-comparison-504f

##### 2. VitePress 的特殊优化

关于是否需要支持 Vue，这里就不得不提到目前基于 Vue 框架的文档方案 VitePress 了，Vue 官网现已接入 VitePress 方案，那基于 VitePress 是否需要做 Islands 架构的优化呢？

答案是不需要。VitePress 内部使用的是 Shell 架构，以 Vue 官网为例：

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761In1r6G4621aibbU1wBlwKwibicuy1NS08PBuHlSAC6gjm0sVqmxdaOAgYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

VitePress 会在 hydrate 的过程中把`正文的静态部分`排除，具体实现原理如下:

- Vue 模板编译阶段，Vue 会对静态虚拟 DOM 节点进行优化，输出 `createStaticVNode` 的格式

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

- 在 Chunk 生成阶段(实现链接)，把内容部分用 `__VP_STATIC_START__` 和 `__VP_STATIC_END__` 标志位包裹

- 在生成打包产物前，针对每个页面打包出两份 JS

- - 一份是包含完整内容的 JS，把标志位去掉即可，比如文件名为`recommend.[hash].js`
  - 另一份是不包含内容的 JS，把标志位及其里面的内容删掉，文件名为`recommend.[hash].lean.js`

由于 VitePress 采用的是 SSG + SPA 模式，其会根据是否为首屏来分发不同的 JS:

- 首屏使用 `.lean.js`，不包含正文部分的 JS，实现 `Partial Client Bundle` + `Partial Hydration`，跟 Islands 架构一样的效果
- 二次页面跳转使用完整的 JS，因为走 SPA 路由跳转，需要拿到完整的页面内容，用 JS 渲染出来。

你可能会问了，在 `.lean.js` 里面，组件的代码都被改了，难道 Vue 在 hydrate 不会发现内容和服务端渲染的 HTML 对应不上进而报错吗？答案是不会，我们可以看看 Vue 里面 createStaticVNode 的实现:

![图片](https://mmbiz.qpic.cn/mmbiz_png/FuxFc4JogFydOSnPJYGD1wJ8FPxj761IB3un6aqVHbzTkCyiah9cBE6m722RbO8lkUxPF4ibbiaudsZBXq5Ylfibsw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

注意第二个传参，里面会记录静态节点的数量，在 hydrate 的过程中对静态节点会特殊处理，直接检查 `staticCount`即节点数量而不是内容，那么对于如下的 VNode 节点来讲 hydrate 仍然是可以成功的:

```js
// recommend.[hash].lean.js
const html = ` A <span>foo</span> B`;
const { vnode, container } = mountWithHydration(html, () =>
  // 保证第二个参数正确即可
  createStaticVNode(``, 3)
);
```

总之，VitePress 利用 Vue 的编译时优化以及内部定制的 Hydrate 方案足以解决传统 SSG 的全量 hydration 问题，采用 Islands 架构意义并不大。

那进一步讲，像 Vue 这种 Shell 优化方案对于包含编译时的前端框架是否通用？这里我们可以先大概总结出 Shell 方案需要满足的条件:

- 模板编译阶段，将静态节点进行特殊标记
- 运行时，支持 hydrate 跳过对静态节点的内容检查

基于上面这两点，其他的代表性编译时框架如`Solid`、`Svelte` 很难实现 Vue 的 Shell 架构(没法标记静态节点)，因此 Shell 方案可以理解为在 Vue 框架下的一个特殊优化了。对于 Vue 外的其它框架方案，仍然可以采用 Islands 进行特定场景的优化。
