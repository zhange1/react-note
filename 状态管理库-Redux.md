#### Redux

概念：Redux是一个JavaScript状态管理容器，提供可预测化的状态管理

用途：提供状态管理，简化高级特性(撤销/重做)，实时编辑，时间旅行，服务端同构

核心原理：

通过createStore,注入Reducer与middleware，生成Store对象

Store对象中的getStore函数用来获取当前状态树，subscribe函数订阅状态数变更，dispatch发送Action

- 三原则：单一数据源、纯函数Reducer，Store是只读的
- 互动方式：Actions→Reducers→Store→View
- 副作用：
  - 是对于调用函数产生附加的影响
  - 怎么解决？
    - 中间件：拦截action，添加副作用
      - 社区方案：Redux-thunk、Redux-Promise、Redux-Observable、Redux-saga
    - reducer：允许reducer层中直接处理副作用
      - 社区方案：react Loop

工程化方案：rematch、dva

优点：

- 结果可预测，代码调试容易
- 代码结构严格易维护
- 模块分离清晰，小函数结构容易编写单元测试
- 开发调试更容易
- 服务端同构容易，传递Store即可完成渲染
- 社区生态繁荣