---
title: redux的个人理解
date: 2019-10-01 22:12:13
tags:
---

## 设计思想
- 视图与状态是一一对应的。
- 所有的状态，保存在一个对象里面。

## 核心原理
- 所有的状态统一挂载到全局对象store上去
- 每一次状态的改变都应该是通过dispatch下发action来触发，action传入对应的reducer中，reducer返回一个新的state并更新store中存放的state
- 通过subscribe订阅对store的监听函数，这些监听通过dispatch发起后依次执行
- 可以通过中间件来对提交的dispatch重写
