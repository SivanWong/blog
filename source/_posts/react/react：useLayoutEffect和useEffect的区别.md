---
title: react：useLayoutEffect和useEffect的区别
Date: 2021-05-11
tags: [react]
categories: react
comments: true
---

## 渲染流程
1. react 在 diff 后，会进入到 commit 阶段，准备把虚拟 DOM 发生的变化映射到真实 DOM 上。
2. 在 commit 阶段的前期，会调用一些生命周期方法，对于类组件来说，需要触发组件的 getSnapshotBeforeUpdate 生命周期，对于函数组件，此时会调度 useEffect 的 create destroy 函数（注意是调度，不是执行）。
3. 在这个阶段，会把使用了 useEffect 组件产生的生命周期函数入列到 React 自己维护的调度队列中，给予一个普通的优先级，让这些生命周期函数异步执行。
4. 随后，就到了 React 把虚拟 DOM 设置到真实 DOM 上的阶段，这个阶段主要调用的函数是 commitWork，commitWork 函数会针对不同的 fiber 节点调用不同的 DOM 的修改方法，比如文本节点和元素节点的修改方法是不一样的。
5. commitWork 如果遇到了类组件的 fiber 节点，不会做任何操作，会直接 return，进行收尾工作，然后去处理下一个节点，这点很容易理解，类组件的 fiber 节点没有对应的真实 DOM 结构，所以就没有相关操作。
6. 但在有了 hooks 以后，函数组件在这个阶段，会同步调用上一次渲染时 useLayoutEffect(create, deps) create 函数返回的 destroy 函数。
7. 注意一个节点在 commitWokr 后，这个时候，我们已经把发生的变化映射到真实 DOM 上了。
8. 但由于 JS 线程和浏览器渲染线程是互斥的，因为 JS 虚拟机还在运行，即使内存中的真实 DOM 已经变化，浏览器也没有立刻渲染到屏幕上。
9. 此时会进行收尾工作，同步执行对应的生命周期方法，我们说的componentDidMount，componentDidUpdate 以及 useLayoutEffect(create, deps) 的 create 函数都是在这个阶段被同步执行。
10. 对于 react 来说，commit 阶段是不可打断的，会一次性把所有需要 commit 的节点全部 commit 完，至此 react 更新完毕，JS 停止执行。
11. 浏览器把发生变化的 DOM 渲染到屏幕上，到此为止 react 仅用一次回流、重绘的代价，就把所有需要更新的 DOM 节点全部更新完成。
12. 浏览器渲染完成后，浏览器通知 react 自己处于空闲阶段，react 开始执行自己调度队列中的任务，此时才开始执行 useEffect(create, deps) 的产生的函数。


## 介绍
### useEffect
 该 Hook 接收一个包含命令式、且可能有副作用代码的函数。在函数组件主体内（这里指在 React 渲染阶段）改变 DOM、添加订阅、设置定时器、记录日志以及执行其他包含副作用的操作都是不被允许的，因为这可能会产生莫名其妙的 bug 并破坏 UI 的一致性。使用 useEffect 完成副作用操作。赋值给 useEffect 的函数会在组件渲染到屏幕之后执行。你可以把 effect 看作从 React 的纯函数式世界通往命令式世界的逃生通道。
 
 - useEffect 是异步执行的
- 执行时机是浏览器将所有变化渲染到屏幕后
 
### useLayoutEffect
其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，useLayoutEffect 内部的更新计划将被同步刷新。

- useLayoutEffect 是同步执行的
- 执行时机是浏览器把内容真正渲染到界面之前
- useLayoutEffect的 create 函数的调用位置、时机都和 componentDidMount，componentDidUpdate 一致，且都是被 React 同步调用，都会阻塞浏览器渲染。
- useLayoutEffect 的 detroy 函数的调用位置、时机与 componentWillUnmount 一致，且都是同步调用。

## 使用方式
这两个函数的使用方式其实非常简单，他们都接受一个函数一个数组，只有在数组里面的值改变的情况下才会再次执行 effect。

```
 useEffect(() => {}, []);
 useLayoutEffect(() => {}, []);
```

## 实际应用
- DOM操作在useLayoutEffect进行
- 其余一般在useEffect进行，如数据初始化

当需要进行DOM操作,并且会改变页面的样式时,就需要用useLayoutEffect,否则可能会出现出现闪屏问题。useLayoutEffect里面的callback函数会在DOM更新完成后立即执行,但是会在浏览器进行任何绘制之前运行完成,阻塞了浏览器的绘制。

可以看到在流程9/10期间，DOM 已经被修改，但但浏览器渲染线程依旧处于被阻塞阶段，所以还没有发生回流、重绘过程。由于内存中的 DOM 已经被修改，通过 useLayoutEffect 可以拿到最新的 DOM 节点，并且在此时对 DOM 进行样式上的修改，假设修改了元素的 height，这些修改会在步骤 11 和 react 做出的更改一起被一次性渲染到屏幕上，依旧只有一次回流、重绘的代价。

如果放在 useEffect 里，useEffect 的函数会在组件渲染到屏幕之后执行，此时对 DOM 进行修改，会触发浏览器再次进行回流、重绘，增加了性能上的损耗。


## 参考
[深入理解 React useLayoutEffect 和 useEffect 的执行时机](https://blog.csdn.net/yunfeihe233/article/details/106616674)