#### 生命周期

###### 挂载阶段

- constructor：
  - 作用：初始化state与绑定函数
  - 注意事项：
    - 不要引入业务逻辑
    - 逐渐被类属性的写法取代
- getDerivedStateFromProps：
  - 作用：让组件在props变化时更新state
  - 注意事项：
    - 并非props变化时调用，传入即可调用
    - 反模式：复制props到state、在props变化后修改state
- UNSAFE_componentWillMount：
  - 作用：过去用于网络请求或绑定函数
  - 注意事项：
    - 标记废弃
    - 异步渲染机制下，该方法可能被多次调用
- Render：
  - 作用：执行渲染
  - 注意事项：
    - 不要使用setState
- componentDidMount：
  - 作用：发起网络请求或绑定事件
  - 注意事项：无特殊点

###### 更新阶段

- UNSAFE_componentWillReceiveProps：
  - 作用：让组件在props变化时更新state
  - 注意事项：
    - 与getDerivedStateFromProps一致
    - 标记废除
- getDerivedStateFromProps:与挂载阶段一致
- shouldComponentUpdate:
  - 作用：通过返回true或false来确定是否需要触发新的渲染
  - 注意事项：
    - 可通过添加判断条件来完成性能优化
    - PureComponent的实现原理
- UNSAFE_componentWillUpdate：
  - 作用：执行更新前的操作
  - 注意事项：
    - 异步渲染机制下，该方法可能多次执行
    - 标记废除
- render：与挂载阶段一致
- getSnapshotBeforeUpdate：
  - 作用：返回一个数据，作为componentDidUpdate的第三个参数传入
  - 注意事项：
    - 可在DOM更新执行某些运算
    - 不可使用setState
- comPonentDidUpdate：
  - 作用：视图更新后执行操作
  - 注意事项：避免使用setState，造成死循环

###### 卸载阶段

- componentWillUnmount：
  - 作用：执行解除事件绑定，取消定时器等清理操作