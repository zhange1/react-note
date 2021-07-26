共同点：

- 使用方式：函数签一致
- 运用效果：处理副作用

不同点：

- 使用场景：
  - Effect:绝大多数场景
  - LayoutEffect:处理DOM调整样式，避免页面闪烁
- 独有能力：
  - Effect:异步处理副作用
  - LayoutEffect：同步处理副作用
- 设计原理：
  - Effect:异步调用
  - LayoutEffect:同步调用
- 未来趋势：暂无变化