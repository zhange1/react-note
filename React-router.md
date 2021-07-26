###### React-router

翻开React Router的代码，你回发现React Router提供了三个库，分别是react-router、react-router-dom、react-router-native

react-router是没有UI层的

- react-router-dom = react-router+DOM UI
- React-router-native = react-router +native UI

DOM版本和Native版本最大限度的复用了同一个底层路由逻辑

- DOM版本中提供的基础路由是BrowserRouter
  - 在React Router中路由通过抽象history库统一管理完成
  - history库支持BrowserHistory与MemoryHistory两种类型
    - 打开源码看一下BrowserHistory实际上调用的是浏览器的History API也就是基础原理部分

##### 工作方式：

- 设计模式：Monorepo、Context
- 关键模块：
  - Context容器
    - Router
    - MemoryRouter
  - 直接消费者，匹配路由
    - Route
    - Redirect
    - Switch
    - ...
  - 与平台管理的功能组件
    - Link
    - NavLink
    - DeepLinking