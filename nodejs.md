## 文件读写

- readFile
- writeFile

## 流和管道

更方便处理数据，比如：fs.createReadStream().pipe(res)

## 路由

在 http.createServer 回调中的 req 对象中存在 url 属性，通过 url 属性判断

## nodemon

## 内存

一个进程最多 2.8g 左右，所以需要集群来起多个进程来负载均衡，只适合轻量计算

## Buffer

像 client 可以通过字符拼接操作字符，server 端就太消耗性能了，需要用到 Buffer 类

## process

进程信息
