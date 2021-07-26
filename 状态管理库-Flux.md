###### **FLUX**

- View视图层，即代码中的React组件
- Store数据层，维护了数据和数据处理的逻辑
- Dispatcher，管理数据流动的中央枢纽
- Action，事件通知，通常用type标识

###### Action调用Dispatcher修改Store内容触发视图改变，视图触发Action调用Dispatcher修改Store内容



概念：使用单向数据流的形式来组合React组件的应用架构

用途：Facebook用其来代替MVC，组织代码，安排内部逻辑

核心原理：

- 四要素：
  - View
  - Store
  - Dispatcher
  - Action
- 互动方式：Action→Dispatcher→Store→View

优点：

- 类比MVC
- 开发中快速了解应用行为
- 数据与业务逻辑统一存放管理
- 简化视图层设计，职责单一
- 大型架构中更容易管理维护代码