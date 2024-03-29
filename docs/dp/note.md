---
title: "高级开发者学习笔记"
date: 2023-02-24T00:01:05+08:00
---

### 单例和全局可变状态

- Singleton: 完整的单例，一个类型只有一个实例
- singleton: 不完整单例，一个类型有一个公用实例，但也可以根据需求创建其它实例
- global mutable state: 全局可变状态会对单元测试造成阻碍，应该尽量避免

### 依赖反转(倒置)

### 注入方法

- 通过闭包：单个的闭包可以看作是只有一个接口声明的协议，它们可以相互转换
- 通过协议：是一组接口的包装

### 注入方式

- 初始化构造器注入
- 属性成员注入
- 方法注入

### 依赖图

依赖图是一种交流工具，设计工具

- 实线空箭头: 表示类继承关系
- 实线实箭头: 表示成员强依赖关系
- 虚线空箭头: 表示协议遵循关系
- 虚线实箭头: 表示成员弱依赖关系
- 方框加类名表示类型
- 方框类名尖括号包裹表示协议类型，即接口。

### 类型组合

可以把不同的类型组合成一个新的类型

### 开闭原则+依赖倒置

### 模块化设计

模块化设计后，一些和特定平台无关的模块，可以单独进行测试，平台解藕也可以实现。
例如：原来在iOS模拟器跑的模块测试可以转移到Mac设备上进行，从而提升效率

模块化设计后，不同的模块开发工作可以合理的并行开发，节省时间成本，提高开发效率

所有的架构都是好的，区别只在于需要解决问题时的具体选择。选择最合适的架构优雅的解决问题才是目的

好的架构是由良好的沟通中产生的

建立良好的需求开发流程

BDD->TDD->UserCases->Architecture->ModularDesign

沟通是关键，明确需求，细化实现，降低风险

尽量少用单例模式，通常单例模式是反模式的，不利于维护，我们使用单例大部分场景，只是为了更方便的调用，仅此而已，但这不利于代码的模块化设计，因为会造成很多耦合点


### 写单元测试

- SUT(System Under Test)
- 测试方法命名：test_method_expectation

单元测试涉及的四个术语：
- Stubbing
- Spying
- Mock
- dummy

善用Result类型，可以简化分支数量

### 测试网络请求的方法

- 端到端：通过检查客户端-服务端之间真实的请求进行，由于依赖真实的网络请求，所以效率比较低，无法单独进行测试。
- 子类Mock：不适合对我们不清楚的类进行子类化
- 协议Mock：UR Loading System
- 请求拦截：URLProtocol/registerClass(AnyClass)/unregisterClass(AnyClass)
