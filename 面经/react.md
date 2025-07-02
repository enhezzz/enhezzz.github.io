1. child 组件如何优化不要每次都渲染

正常情况下，react 会在父组件 re-render 时 render 组件

通过 memo 化，默认是通过 shallow equality 比较，可以通过传入 comparison func 自定义比较

caveat:

- 深层比较是相当慢的，如果有人稍后更改数据结构，可能会冻结应用数秒。
- 使用浏览器开发人员工具中的 Performance 面板来确保比较函数实际上比 re-render 组件要快。
