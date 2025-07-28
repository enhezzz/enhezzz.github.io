## web worker

### 应用场景

1. 大数据计算
2. 结合离屏 canvas， 提高 Canvas 运行速度

```javascript
let worker = new Worker();
let htmlCanvas = document.getElementById("myCanvas");
let offscreen = htmlCanvas.transferControlToOffscreen();
// 注意：第二个参数不能省略
worker.postMessage({ canvas: offscreen }, [offscreen]);
```

### 限制

1. 在 Worker 线程的运行环境中没有 window 全局对象，也无法访问 DOM 对象
2. Worker 中只能获取到部分浏览器提供的 API，如定时器、navigator、location、XMLHttpRequest 等
3. 每个线程运行在完全独立的环境中，需要通过 postMessage、 message 事件机制来实现的线程之间的通信

## 之前不常用方法

### window.requestIdleCallback

- 可作为后台或者低优先级任务在浏览器空闲阶段执行，默认不会影响到异步任务如用户输入、animation 的执行
- 默认按照 first-in-first-out 顺序执行
- 可执行一个超时时间，当 idlecallback 在 timeout 后还没执行的话会被推入到事件循环宏任务队列中
- callback 传入了两个参数
  - didTimeout，判断是否是因为 timeout 时间到了而被执行的
  - timeRemaining()，返回当前空闲时间段剩余的预估毫秒数，可以基于此来判断是否还要执行其他工作或者将工作交给下一个 idleCallback

### navigator.sendBeacon

- 为 analytics 而设计
- 发送小数据量的 post 请求
- 可发送 An ArrayBuffer, a TypedArray, a DataView, a Blob, a string literal or object, a FormData or a URLSearchParams object

### Transferable objects

可转移对象是拥有可以从一个上下文转移到另一个上下文的资源的对象，确保资源一次只能在一个上下文中使用。转移后，原始对象不再可用；它不再指向传输的资源，任何读取或写入对象的尝试都会引发异常。

```javascript
// Create an 8MB "file" and fill it. 8MB = 1024 * 1024 * 8 B
const uInt8Array = new Uint8Array(1024 * 1024 * 8).map((v, i) => i);
console.log(uInt8Array.byteLength); // 8388608

// Transfer the underlying buffer to a worker
worker.postMessage(uInt8Array, [uInt8Array.buffer]);
console.log(uInt8Array.byteLength); // 0
```

```javascript
const original = new Uint8Array(1024);
const clone = structuredClone(original);
console.log(original.byteLength); // 1024
console.log(clone.byteLength); // 1024

original[0] = 1;
console.log(clone[0]); // 0

// Transferring the Uint8Array would throw an exception as it is not a transferable object
// const transferred = structuredClone(original, {transfer: [original]});

// We can transfer Uint8Array.buffer.
const transferred = structuredClone(original, { transfer: [original.buffer] });
console.log(transferred.byteLength); // 1024
console.log(transferred[0]); // 1

// After transferring Uint8Array.buffer cannot be used.
console.log(original.byteLength); // 0
```

### message channel

```javascript
// 主页面代码
const iframe = document.getElementById("myIframe");
const channel = new MessageChannel(); // 创建通信通道

// 监听来自 iframe 的消息（通过 port1）
channel.port1.onmessage = (event) => {
  console.log("主页面收到消息:", event.data);
};

// 将 port2 传递给 iframe（关键步骤！）
iframe.contentWindow.postMessage(
  "INIT_PORT", // 消息内容（可以是任意标识）
  "*", // 目标源（* 表示不限制）
  [channel.port2] // 第三个参数：传输的端口对象
);
```

```javascript
// iframe 内部代码
window.addEventListener("message", (event) => {
  // 1. 验证消息来源（安全措施）
  if (event.origin !== "https://your-parent-origin.com") return;

  // 2. 检查是否为端口初始化消息
  if (event.data === "INIT_PORT") {
    // 3. 从 event.ports[0] 获取传递的端口
    const port = event.ports[0];

    // 4. 启动端口监听（必须！）
    port.start();

    // 5. 监听来自主页面的消息
    port.onmessage = (e) => {
      console.log("iframe收到消息:", e.data);
    };

    // 6. 通过端口向主页面发送消息
    port.postMessage("IFRAME_READY");
  }
});
```

## sourceMap

在编译过程中，会生成一个 .map 文件，一般用于代码调试和错误监控。

- 包含了源代码、编译后的代码、以及它们之间的映射关系。
- 编译后的文件通常会在文件末尾添加一个注释，指向 SourceMap 文件的位置。
  - // # sourceMappingURL=example.js.map
- 当在浏览器开发者工具调试时，浏览器会读取这行注释并加载对应的 SourceMap 文件
