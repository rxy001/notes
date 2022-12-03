### React 源码解析系列之一

***首先声明本系列所使用的源码为 18.2.0，legacy mode。[此仓库](https://github.com/rxy001/toy-react) 复制并简化了 react 中的源码，翻译源码中部分注释，并添加一些个人理解，建议搭配食用。其次本文只考虑函数式组件，有关类组件的源码都将忽略***

当查看 react repo 时会发现其中的 packages 非常多，在 web 端使用时，核心包有 react、react-dom、react-reconciler 及 scheduler。

#### react、react-dom、react-reconciler、scheduler

- *React* 负责对外暴露 API ，React Element（即 Virtual DOM）的创建。

- *ReactDOM* 作为 React 的 DOM 渲染的入口点，负责 DOM 的创建、更新等操作。

- *ReactReconciler* 是 React 核心所在，众所周知的 fiber 架构即其核心算法。

- *scheduler* 为 ReactReconciler 提供任务调度的实现。

以上4个包，主要研究对象是 **react-reconciler**。

---

```js
const element = <h1>Hello, world!</h1>;
```

在使用 React 时，通常都会搭配 JSX， JSX 语法类似于 HTML，是一个 JavaScript 的语法扩展。编译器会将其转换为浏览器可以理解的 React 函数调用，旧的 JSX 转化会把 JSX 转化为`React.createElement(...)` 调用 （17 版本以下）。

```js
const element = React.createElement('h1', null, 'Hello world');
```

新的 JSX 转化自动从 React 的 package 中引入新的入口函数并调用，因此源代码中不需要再主动引入 React 。

```js
// 由编译器引入（禁止自己引入！）
import {jsx as _jsx} from 'react/jsx-runtime';

const element = _jsx('h1', { children: 'Hello world' });

```

`_jsx` 函数返回值即是 React Element：

```js
{
    $$typeof: ReactElementSymbol,
    type: "h1",
    key: null,
    props: {
      children: 'Hello world'
    },
    ref: null,
}
```

---

在入口文件中，都会先调用 `React.render(document.getElementById("root"))`，将 root 作为根节点。

> **为什么需要 root 节点 ？**
>
> 在 react-reconciler 中，所有的组件以及 DOM 元素都将转化为 fiber ，fiber 通过 return、child、sibling 三个指针表示了其中的关系，以 root 为起始节点形成链表，因此使得能够递归遍历 fiber tree。

`React.render()` 过程会先调用 `createFiberRoot ` 创建 `fiberRoot` 及 `rootFiber`。

```js
function createFiberRoot(container, tag, initialChildren = null) {
  const root = new FiberRootNode(container, tag);

  const uninitializedFiber = createHostRootFiber(tag);

  root.current = uninitializedFiber;
  uninitializedFiber.stateNode = root;

  uninitializedFiber.memoizedState = {
    element: initialChildren,
  };

  initializeUpdateQueue(uninitializedFiber);

  return root;
}
```

> **fiberRoot 与 rootFiber 区别**
>
> fiberRoot 会保存在 `container._reactRootContainer`，使得同一 container 多次调用 render 时只会创建一次 fiberRoot。fiberRoot 
