> [原文地址](https://www.51cto.com/article/750028.html)

# Signals 在 JavaScript 中的应用

最近，"Signals"成为了前端备受关注的话题。很多国外的大佬都发文表示 Signals 是前端框架的未来。同时，尤大也在 Vue 官网上添加了"Connection to Signals"部分。此外，包括 Solid、Angular、Preact、Qwik 和 Vue 等多个前端框架都已经开始实现 Signals。

作为一名 FE，如果你和我之前一样还不是很了解 Signals，那么这篇文章或许可以帮助你更好地了解一下这个技术。本文将介绍 Signals 的历史、概念和优势。

## 一、发展历史

自从声明式 JavaScript 框架问世以来，Signals 机制一直存在。随着时间的推移，它采用了许多不同的名称，经历了多年的流行和消失。

在声明式 JavaScript 框架中，组件是声明其输出的单元，可以被动态地渲染和组合。这种方式的优点是，它允许开发人员集中精力于组件的输出，而无需担心组件如何被渲染和更新。这种抽象方式也使得组件更容易被复用，并且更容易理解和测试。

然而，这种抽象也带来了一些挑战。其中一个挑战是组件之间如何通信和共享状态。这些问题可能导致代码变得笨重，难以维护，并且在复杂的应用程序中容易出现混乱。因此，开发人员需要一种更灵活、更强大的通信机制来解决这些问题。

在这种情况下，Signals 机制成为了一个有用的解决方案。Signals 机制允许组件在不直接引用其他组件的情况下通信，并且能够更灵活地传递消息和状态。这些机制可以是事件、回调、Promise 或其他异步机制。它们可以被用来处理各种不同的场景，例如用户交互、网络请求和状态更改等。

Signals 机制还具有许多其他优点。它们可以提高应用程序的可维护性和可扩展性，并且可以帮助开发人员更好地理解和调试代码。此外，由于 Signals 机制允许组件之间松散耦合，因此它们也有助于提高代码的可重用性。

#### 1.1 早期实现

有时令人惊讶的是，多个团队几乎在同一时间达成了相似的解决方案。声明式 JavaScript 框架的开端有三个版本：Knockout.js（2010 年 7 月），Backbone.js（2010 年 10 月）和 Angular.js（2010 年 10 月)。

Angular 的脏检查，Backbone 的模型驱动的重渲染，以及 Knockout 的细粒度更新。每一个都略有不同，但最终都将成为我们今天管理状态和更新 DOM 的基础。

#### 1.2 数据绑定

Angular.js 里面常用的模式叫作数据绑定。数据绑定是将部分状态（state）附加到视图树（view tree）某个特定部分的一个方法。可以做到的一个强大的事情是使其成为双向的。因此，我们可以让状态更新 DOM，反过来，DOM 事件自动更新状态，所有这些都是以一种简单的声明方式进行的。但是如果滥用也会出现问题，在 Angular 中，如果不知道有什么变化，就会对整个树进行肮脏的检查，向上传播可能会导致它发生多次。

### 1.3 Mobx

之后就是 react 的时代，react 对状态管理没有太多的限制。MobX 就是这种解决方案。它强调一致性和无障碍传播。也就是说，对于任何给定的变化，系统的每一部分都只运行一次，而且是以适当的顺序同步运行。

它通过将先前方案中典型的基于 push 的响应式换成 push-pull 混合系统来做到这一点。变化的通知被推送出去，但派生状态的执行被推迟到读取它的地方。

![图片](https://s4.51cto.com/oss/202303/24/29d193d65b3364206e15858e078668aab9294d.png)

#### 1.4 Vue

Vue(2014) 也为今天的发展提供了巨大的贡献。除了在优化一致性方面与 MobX 保持一致外，Vue 从一开始就将「细粒度」的响应性作为其核心。

虽然 Vue 与 React 共享虚拟 DOM 的使用，但响应性是一流的，这意味着它首先作为一种内部机制与框架一起开发，以支持其 Options API，并在过去几年中，成为 Composition API 的核心 （2020）。

Vue 通过调度任务，将 pull / push 机制向前推进了一步。默认情况下，Vue 的修改不会立马被执行，而是要等到下一个微任务才会执行。

然而，这种调度也可以用来做一些其他的事情，比如 keep-alive，以及 Suspense。甚至像并发渲染这样的事情也可以用这种方法来实现，真正展示了如何获得基于 pull 和 push 的两种方法的最佳效果。

## 二、为什么是 Signals

Signals 的独特之处在于状态更改会以最有效的方式来自动更新组件和 UI。Signals 基于自动状态绑定和依赖跟踪提供了出色的工效，并具有针对虚拟 DOM 优化的独特实现。

#### 2.1 状态管理的困境

随着应用越来越复杂，项目中的组件也会越来越多，需要管理的状态也越来越多。

为了实现组件状态共享，一般需要将状态提升到组件的共同的祖先组件里面，通过 props 往下传递，带来的问题就是更新时会导致所有子组件跟着更新，需要配合 memo 和 useMemo 来优化性能。

虽然这听起来还挺合理，但随着项目代码的增加，我们很难确定这些优化应该放到哪里。

即使添加了 memoization，也常常因为依赖值不稳定变得无效，由于 Hooks 没有可以用于分析的显式依赖关系树，所以也没法使用工具来找到原因。

![图片](https://s9.51cto.com/oss/202303/24/694feda2597af1105d4220b5375598bb69c416.png)

另一种解决方案就是放到 Context 上面，子组件作为消费者自行通过 useContext 来获取需要的状态。

但是有一个问题，只有传给 Provider 的值才能被更新，而且只能作为一个整体来更新，无法做到细粒度的更新。

为了处理这个问题，只能将 Context 进行拆分，业务逻辑又不可避免地会依赖多个 Context，这样就会出现 Context 套娃现象。

![图片](https://s7.51cto.com/oss/202303/24/e50fc9b885342c362db6785be9d4c381bf47ce.png)

#### 2.2 通向未来的 Signals

Signal 的核心是一个通过.value 属性 来保存值的对象。它有一个重要特征，那就是 Signal 对象的值可以改变，但 Signal 本身始终保持不变。

复制

```none
import { signal } from "@preact/signals";

const count = signal(0);

// Read a signal’s value by accessing .value:
console.log(count.value);   // 0

// Update a signal’s value:
count.value += 1;

// The signal's value has changed:
console.log(count.value);  // 11.2.3.4.5.6.7.8.9.10.11.12.
```

在 Preact 中，当 Signal 作为 props 或 context 向下传递时，传递的是对 Signal 的引用。这样就可以在不重新渲染组件的情况下更新 Signal，因为传给组件的是 Signal 对象而不是它的值。

这让我们可以跳过所有昂贵的渲染工作，立即跳到任意访问 signal.value 属性的组件。

![图片](https://s9.51cto.com/oss/202303/24/35314080876e2f3bae57846a1b66bde6a5e066.png)

Signals 具有第二个重要特征，即它们会跟踪其值何时被访问以及何时被更新。在 Preact 中，当 Signal 的值发生变化时，从组件内访问 Signal 的属性会自动重新渲染组件。

通过 Preact 的使用，我们可以总结 Signals 几点特点：1、感觉上像是使用原始数据结构 2、能根据值的变化自动更新 3、直接更新 DOM （换句话来说无 VDOM） 4、没有依赖数组

## 三、在 SolidJS 中的使用

复制

```none
const Greeting = (props) => (
  <>Hi <span>{props.name}</span></>
);

const App(() => {
  const [visible, setVisible] = createSignal(false),
    [name, setName] = createSignal("Josephine");

  return (
    <div notallow={() => setName("Geraldine")}>{
      visible() && <Greeting name={ name } />
    }</div>
  );
});

render(App, document.body);1.2.3.4.5.6.7.8.9.10.11.12.13.14.15.16.
```

可以看到 SolidJS 响应式也是 Signal 作为基础，createSignal 既可以用于组件内，也可以用于组件外，这个跟 Preact 中类似。一方面可以将 Signal 作为组件的 local state，也可以定义为 global State。与前面类似，SolidJS 中也有以下相似点：

- 响应式细粒度更新
- 无需定义 dependencies
- 惰性取值

SolidJS 与 Mobx 和 Vue 的响应式非常相似，但是不会处理 VDOM，而是直接更新 DOM。

## 四、手动实现一个

响应式状态管理三要素，Signals、Reactions、Derivations

复制

```none
// context 包含Reactions中的执行方法和Signal依赖
const context = [];

const createSignal = (value) => {
  const subscriptions = new Set();
  const readFn = () => {
    const running = context.pop();
    if (running) {
      subscriptions.add({
        execute: running.execute
      });
      running.deps.add(subscriptions);
    }
    return value;
  };
  const writeFn = (newValue) => {
    value = newValue;
    for (const sub of [...subscriptions]) {
      sub.execute();
    }
  };
  return [readFn, writeFn];
};

const createEffect = (fn) => {
  const execute = () => {
    running.deps.clear();
    context.push(running);
    try {
      fn();
    } finally {
      context.pop(running);
    }
  };

  const running = {
    execute,
    deps: new Set()
  };
  execute();
};

const createMemo = (fn) => {
  const [memo, setMemo] = createSignal();
  createEffect(() => setMemo(fn()));
  return memo;
};

const [name, setName] = createSignal("a");
const fullName = createMemo(() => {
  return "c-" + name();
});
createEffect(() => console.log(name(), fullName()));
setName("b");1.2.3.4.5.6.7.8.9.10.11.12.13.14.15.16.17.18.19.20.21.22.23.24.25.26.27.28.29.30.31.32.33.34.35.36.37.38.39.40.41.42.43.44.45.46.47.48.49.50.51.52.53.54.
```

Signals 是一个基础的数据更新与读取，Reactions 是可以追踪订阅到 Signals 的变化，所以在 Reactions 函数里设置 Derivations 的值。

#### 五、最后

本文是学习 Signals 的一些记录。希望能通过介绍响应式状态管理的一些历史和理念，让你对状态管理有全面的认识，如果感觉本文介绍的不够详细，可以阅读下面的引用原文。

#### 六、引用

1. [https://dev.to/this-is-learning/the-evolution-of-signals-in-javascript-8ob](https://dev.to/this-is-learning/the-evolution-of-signals-in-javascript-8ob)
2. [https://dev.to/this-is-learning/react-vs-signals-10-years-later-3k71](https://dev.to/this-is-learning/react-vs-signals-10-years-later-3k71)
3. [https://mp.weixin.qq.com/s/Tn0rbkCdFw4f-3ihKUEYQA](https://mp.weixin.qq.com/s/Tn0rbkCdFw4f-3ihKUEYQA)
4. [https://indepth.dev/posts/1289/solidjs-reactivity-to-rendering](https://indepth.dev/posts/1289/solidjs-reactivity-to-rendering)
5. [https://preactjs.com/guide/v10/signals/](https://preactjs.com/guide/v10/signals/)
6. [https://preactjs.com/blog/introducing-signals](https://preactjs.com/blog/introducing-signals)
