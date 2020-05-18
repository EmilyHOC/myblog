---
title: react知识点整理
date: 2020-05-18 14:59:42
tags: react 
---

##### react和vue的优势

比较好的文章：

https://libin1991.github.io/2019/09/07/%E9%9D%A2%E8%AF%95%E5%AE%98%EF%BC%9AVue-%E5%92%8C-React-%E7%9A%84%E4%BC%98%E7%82%B9%E5%88%86%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F%E4%BD%A0%E5%BA%94%E8%AF%A5%E8%BF%99%E4%B9%88%E7%AD%94%EF%BC%81/

简单来说vue和react的区别：

1. Vue进行数据拦截，它对数据的变化更敏感。React是函数式编程，它进行局部重新刷新，简单粗暴，但是它不知道怎么去刷新，触发局部的变化是由开发者手动setState完成的。相比之下，vue采用依赖追踪，默认就是优化状态，动了多少数据，就触发多少更新,react对数据变化毫无感知，他就提供react.craeteElement调用已生成的Virtual dom,另外react为了弥补不必要的更新，会对setState进行合并，因此setState有时候会是异步更新，有时候会是同步更新
2. react的事件暴露给开发者的不是原生事件，而是合成事件，对所有事件 都进行了代理，将所有事件都绑定document上
3. react中事件处理函数中的this默认不指向组件，vue中的this默认指向组件

优势：

1. 单项数据流
2. 组件化（class componet和function component）
3. 结构更清晰

#####  如何做技术选型

##### 关于脚手架，用途，优势

##### 如何定义一个react组件

##### JSX语法规则

##### react父子组件通信

##### 如何优化一个react项目

##### JQuery和react的区别

##### 什么是单向数据流

##### 函数式编程的好处

##### 什么是属性接收强校验

##### prop、state和render之间的关系

##### react如何实现响应式

##### 为什么要引入虚拟DOM

##### 虚拟DOM的Diff算法

##### 为什么不用index做key值

##### react中ref的使用

##### react的生命周期有哪些

##### 哪个生命周期可以用来优化

##### 关于axios放在哪

##### 关于如何写一个假数据

##### 关于react-transition-group的使用

##### 为什么react要配合redux来使用

##### redux工作流程

##### redux设计使用原则

##### redux核心API

##### **react异步数据如ajax请求应该放在哪个生命周期？**

对于同步的状态改变，是可以放在componentWillMount，对于异步的，最好好放在componentDidMount。但如果此时有若干细节需要处理，比如你的组件需要渲染子组件，而且子组件取决于父组件的某个属性，那么在子组件的componentDidMount中进行处理会有问题：因为此时父组件中对应的属性可能还没有完整获取，因此就让其在子组件的componentDidUpdate中处理。