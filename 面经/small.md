## 灏仟亿

1. ![alt text](original_1751526588865_66af91e1b7295ed518497809186b0c72.png)

2. 自动分包的方式？splitChunkpuglin

3. Vizier-cli 做了什么？如果需要拉取一段代码到指定文件区域怎么实现？同时还要校验依赖和验证运行是否报错呢？

4. yarn pnpm 有了解吗

5. 为什么要改为 ice.js

6. monoRepo

- 好处
  - 可见性（Visibility）：每个人都可以看到其他人的代码，这样可以带来更好的协作和跨团队贡献
  - 更简单的依赖关系管理（Simpler dependency management）：共享依赖关系很简单，因为所有模块都托管在同一个存储库中，因此都不需要包管理器。
  - 唯一依赖源（Single source of truth）：每个依赖只有一个版本，意味着没有版本冲突，没有依赖地狱。
  - 一致性（Consistency）：当你把所有代码库放在一个地方时，执行代码质量标准和统一的风格会更容易。
  - 共享时间线（Shared timeline）：API 或共享库的变更会立即被暴露出来，迫使不同团队提前沟通合作，每个人都得努力跟上变化。
  - 统一的 CI/CD/构建
- 坏处
  - 性能差（Bad performance）：单一代码库难以扩大规模，像 git blame 这样的命令可能会不合理的花费很长时间执行，IDE 也开始变得缓慢，生产力受到影响，对每个提交测试整个 repo 变得不可行。
  - 破坏主线（Broken main/master）：主线损坏会影响到在单一代码库中工作的每个人
  - 学习曲线（Learning curve）：如果代码库包含了许多紧密耦合的项目，那么新成员的学习曲线会更陡峭。
  - 大量的数据（Large volumes of data）：单一代码库每天都要处理大量的数据和提交。
  - 所有权（Ownership）：维护文件的所有权更有挑战性，因为像 Git 或 Mercurial 这样的系统没有内置的目录权限。

与技术无关，而是与工作文化和沟通有关。

7. 模块联邦和微前端的区别

8. class 组件和 function 组件的区别，为啥弃用 class 组件

| 特性         | Class 组件                               | Function 组件                    |
| ------------ | ---------------------------------------- | -------------------------------- |
| 定义方式     | 继承 React.Component 的 ES6 Class        | 普通 JavaScript 函数             |
| 状态管理     | 使用 this.state 和 this.setState()       | 使用 useState Hook               |
| 生命周期方法 | componentDidMount, componentDidUpdate 等 | 使用 useEffect Hook 模拟         |
| 代码复杂度   | 模板代码较多（构造函数、render 等）      | 更简洁，逻辑集中                 |
| 逻辑复用     | 通过 HOC（高阶组件）或 Render Props      | 通过自定义 Hook（如 useFetch）   |
| 性能优化     | PureComponent 或 shouldComponentUpdate   | React.memo + useMemo/useCallback |

新特性（如并发模式）优先适配 Function 组件。

9. http2 为啥没有对头阻塞？如何判断 tcp 数据段是否属于同一个请求？

10. 从传输、构建等方面说说性能优化

11. ts 比 js 好在哪里
