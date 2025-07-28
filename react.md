## error-boundary

- 封装一个 error-boundary 组件，通过是通过结合 class 组件的 getDerivedStateFromError 和 componentDidCatch 来实现，然后作为最外层组件进行包裹，用来捕获子级组件在渲染时抛出的错误
- If you want to perform a side effect (for example, to call an analytics service), you need to also implement componentDidCatch.
- 通过[react-error-boundary](https://github.com/bvaughn/react-error-boundary)代替方案
