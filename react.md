## error-boundary

- 封装一个 error-boundary 组件，通过是通过结合 class 组件的 getDerivedStateFromError 和 componentDidCatch 来实现，然后作为最外层组件进行包裹，用来捕获子级组件在渲染时抛出的错误
- If you want to perform a side effect (for example, to call an analytics service), you need to also implement componentDidCatch.
- 通过[react-error-boundary](https://github.com/bvaughn/react-error-boundary)代替方案

## fiber

> https://juejin.cn/post/6943896410987659277?searchId=20250727163409DC2A4369AD9D8FC6F857

## 为什么 vue 不需要 fiber 架构？

- react 知道哪个组件触发了更新，但是不知道哪些子组件会受到影响。因此 react 需要生成该组件下的所有虚拟 DOM 结构，与原本的虚拟 DOM 结构进行对比，找出变动的部分。
- 在 vue 中，一切影响页面内容的数据都应该是响应式的，vue 通过拦截响应式数据的修改，知道哪些组件应该被修改。不需要遍历所有子树。vue 的 diff 算法是对组件内部的 diff，如果存在子组件，会判断子组件上与渲染相关的属性是否发生变化（props 属性也是响应式属性），无需变化的化则复用原本的 DOM，不会处理子组件。

## 为什么 props 是只读的？

因为 React 的设计中，是根据单向数据流的设计模式，使状态可预测。如果子组件可以修改 props 的值，那么就会造成状态的不可预测性，给后续的维护及调试带来很大的困扰。
