---
title: Reat源码学习(1) --- React 16的架构
date: 2021-05-05 14:07:55
tags:
---

### React16的架构
* 异步可中断更新
  更新在执行过程中可能会被打断（浏览器时间分片用尽或有更高优任务插队），当可以继续执行时恢复之前执行的中间状态
* 架构分层
  1. Scheduler（调度器）：调度任务的优先级，优先级高的进入**Reconciler**
  2. Reconciler（协调器）：负责找出变化的组件
  3. Renderer（渲染器）：负责将变化的组件渲染到页面上
* Fiber的含义
  1. 架构层面，React15的`Reconciler`采用递归方式执行，数据保存在递归调用栈中，所以被称为`Stack Reconciler`。React16的Reconciler基于Fiber节点实现，被称为`Fiber Reconciler`。
  2. 从静态数据结构来说，每个`Fiber 节点`对应一个`React Element` 保存了改组件的类型，对应的DOM节点等信息；
  3. 从动态工作单元来说，每个Fiber节点保存了本次更新中该组件改变的状态、要执行的工作（需要被删除/被插入页面中/被更新...）


