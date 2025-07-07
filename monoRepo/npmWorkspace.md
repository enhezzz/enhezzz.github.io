# npm workspace

## Description

工作区是一个通用术语，指的是 npm cli 中的一组特性，这些特性支持从一个顶级根包中管理本地文件系统中的多个包。

每一个 package 也称为 workspace,在根目录执行`npm install`时会将每个 package 自动 被 symbolLink 到根目录下的 node_modules

## Defining workspaces

```javascript
{
  "name": "my-workspaces-powered-project",
  "workspaces": ["packages/a"]
}
```

## Getting started with workspaces

您可以使用 npm init 自动执行定义新 workspaces 所需的步骤。例如，在一个已经定义了 package.json 的项目中，你可以运行：

```shell
npm init -w ./packages/a
```

此命令将创建丢失的文件夹和一个新的 package.json 文件（如果需要），同时确保正确配置根项目 package.json 的“workspaces”属性。

## Adding dependencies to a workspace

```shell
npm install xxx -w a
```

## Using workspaces

```javascript
// ./packages/a/index.js
module.exports = "a";

// ./lib/index.js
const moduleA = require("a");
console.log(moduleA); // -> a
```

## Running commands in the context of workspaces

```shell
# run the given command in the context of that specific workspace.
npm run test --workspace=a
```

```shell
# run the command for each workspace within the 'packages' folder
npm run test --workspace=packages
```

```shell
# running that command in the context of all configured workspaces.
npm run test --workspaces
```

## Ignoring missing scripts

By running the command with the --if-present flag, npm will ignore workspaces missing target script.

```shell
npm run test --workspaces --if-present
```
