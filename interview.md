1. qiankun和自研容器的区别
 style isolation；preloading；accessing sub-applications is as simple as using an iframe.

2. iframe
通常情况下，出于安全考虑，一个iframe无法直接访问其宿主页面（即父页面）的JavaScript变量，除非iframe和宿主页面位于同一个域下。如果iframe和宿主页面同源（即协议、域名和端口号都相同），则可以通过JavaScript的`parent`关键字来访问宿主页面的全局变量。例如，如果宿主页面有一个全局变量`var example = "Hello, world!"`，那么在同源的iframe中可以通过`parent.example`来访问这个变量。

如果iframe和宿主页面不同源，出于跨域安全策略的限制，直接访问是不被允许的。不过，可以通过`window.postMessage`方法实现跨域通信，这种方式允许不同源的页面相互发送消息

 **通过`window.postMessage`方法进行跨源通信**：这是一种安全的通信方式，可以用来在不同源之间发送消息。父页面和`iframe`都可以使用`window.postMessage`方法发送消息，并通过监听`message`事件来接收消息。

   - 父页面向`iframe`发送消息：
     ```javascript
     document.getElementById("myIframe").contentWindow.postMessage('Hello', 'https://iframe-source.com');
     ```
   - `iframe`向父页面发送消息：
     ```javascript
     window.parent.postMessage('Hello Parent', 'https://parent-source.com');
     ```

3. GUI渲染线程与JS线程是互斥的。所以JS 脚本执行和浏览器布局、绘制不能同时执行。