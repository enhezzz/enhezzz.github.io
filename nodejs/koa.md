# koa

背后也是由 express 团队开发，利用 async 函数抛弃回调函数，增强了错误处理，并且核心没有包含任何中间件

## Application

Koa 应用程序是一个包含中间件函数数组的对象，这些函数在请求时以类似堆栈的方式组合和执行。

### Cascading

Koa 调用“下游”，然后控制流回“上游”。

当中间件调用 next（）时，该函数将挂起并将控制传递给下一个定义的中间件。在没有更多的中间件要执行下游之后，堆栈将展开，每个中间件将恢复执行其上游行为。

### Settings

### app.listen(...)

app.listen（…）方法只是用于以下内容的糖

```javascript
const http = require("http");
const Koa = require("koa");
const app = new Koa();
http.createServer(app.callback()).listen(3000);
```

### app.callback()

返回一个适用于 http.createServer（）方法的回调函数来处理请求。您还可以使用此回调函数在 Connect/Express 应用程序中挂载您的 Koa 应用程序。

### app.use(function)

将给定的中间件功能添加到此应用程序

### app.keys=

设置签名 cookie 密钥。

### app.context

App.context 是创建 CTX 的原型。你可以通过编辑 app.context 给 ctx 添加额外的属性。

挂载的应用程序目前使用父应用程序的 ctx 和设置。因此，挂载的应用程序实际上只是一组中间件。

```javascript
app.context.db = db();

app.use(async (ctx) => {
  console.log(ctx.db);
});
```

### Error Handling

默认情况下，将所有错误输出到 stderr，除非 app.silent 为 true。当 err.status 为 404 或 err.expose 为 true 时，默认错误处理程序也不会输出错误。要执行自定义错误处理逻辑，如集中式日志记录，您可以添加一个“错误”事件监听器：

```javascript
app.on("error", (err) => {
  log.error("server error", err);
});
```

如果 req/res 周期出现错误，并且无法响应客户端，则还会传递 Context 实例：

```javascript
app.on("error", (err, ctx) => {
  log.error("server error", err, ctx);
});
```

当错误发生时，它仍然可以响应客户端，也就是说没有数据写入套接字，Koa 将适当地响应 500“内部服务器错误”。在任何一种情况下，都会发出一个应用级的“错误”，用于记录日志。

## Context

Koa 上下文将 Node 的请求和响应对象封装到单个对象中，每个请求都创建一个上下文

为了方便起见，许多上下文的访问器和方法只是委托给它们的 ctx.request 或 ctx.response 等价物，在其他方面是相同的。例如，ctx.type 和 ctx.length 委托给响应对象，ctx.path 和 ctx.method 委托给请求。

### API

#### ctx.req

Node's request object.

#### ctx.res

Node's response object.

不支持绕过 Koa 的响应处理。避免使用以下 Node 属性:

- res.statusCode
- res.writeHead()
- res.write()
- res.end()

#### ctx.request

A Koa Request object.

#### ctx.response

A Koa Response object.

#### ctx.state

推荐的命名空间，用于将信息传递到中间件和前端视图，可以在上下文中保存一些信息

```javascript
ctx.state.user = await User.find(id);
```

## Koa 与 Connect/Express 有哪些不同?

Koa 旨在“修复和替换 node”，而 Express 旨在“增强 node”。Koa 使用 promise 和 async 函数来摆脱应用程序的回调地狱，并简化错误处理。它公开了自己的 ctx.request 和 ctx.response 对象，而不是 Node 的 req 和 res 对象。

### 基于 Promises 的控制流程, 没有回调地狱。

### Koa 非常精简,不含任何中间件

| Feature           | Koa    | Express |
| ----------------- | ------ | ------- |
| Middleware Kernel | ✓      | ✓       |
| Routing           | ------ | ✓       |
| Templating        | ------ | ✓       |
| Sending Files     | ------ | ✓       |
| JSONP             | ------ | ✓       |

### Api

#### request

##### request.fresh

检查请求缓存是否“新鲜”，即内容是否没有更改。此方法用于 If None Match/ETag 和 If Modified Since 和 Last Modified 之间的缓存协商。它应该在设置一个或多个响应标头后被引用。

```javascript
// freshness check requires status 20x or 304
ctx.status = 200;
ctx.set("ETag", "123");

// cache is ok
if (ctx.fresh) {
  ctx.status = 304;
  return;
}

// cache is stale
// fetch new data
ctx.body = await db.find("something");
```

##### request.is(types...)

检查传入的请求是否包含“Content-Type”报头字段，是否包含任何给定的 mime 类型。如果没有请求体，则返回 null。如果没有内容类型，或者匹配失败，则返回 false。否则，它返回匹配的内容类型。

如果你想确保只有图像被发送到给定的路由:

```javascript
if (ctx.is("image/*")) {
  // process
} else {
  ctx.throw(415, "images only!");
}
```
