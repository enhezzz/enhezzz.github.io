# webpack

## optimization

### sideEffects = true

当指定的npm包的export没有被使用过时，并且该包的package.json中包含"sideEffects": false标记时，那么就会跳过该module

隐式：当模块只包含无副作用语句时，将其标记为无副作用。

### splitChunks

### The Manifest

In a typical application or site built with webpack, there are three main types of code:

The source code you, and maybe your team, have written.
Any third-party library or "vendor" code your source is dependent on.
A webpack runtime and manifest that conducts the interaction of all modules.

Run time is all the code webpack needs to connect your modularized application using Manifest that has detaile note on all your module.

### Why webpack
have something that will not only let us write modules but also support any module format (at least until we get to ESM) and handle resources and assets at the same time

### Under The Hood

During the bundling process, modules are combined into chunks. Chunks combine into chunk groups and form a graph (ChunkGraph) interconnected through modules. When you describe an entry point - under the hood, you create a chunk group with one chunk.

There might be more than one chunk in a chunk group. For example SplitChunksPlugin splits a chunk into one or more chunks.

#### Chunks

Chunks come in two forms

* initial is the main chunk for the entry point. This chunk contains all the modules and their dependencies that you specify for an entry point.
* non-initial is a chunk that may be lazy-loaded. It may appear when dynamic import or SplitChunksPlugin is being used.

## guide

### getting started

We can adjust our package.json file in order to make sure we mark our package as private, as well as removing the main entry. This is to prevent an accidental publish of your code.
```
 {
   "name": "webpack-demo",
   "version": "1.0.0",
   "description": "",
   "private": true,
   "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1"
   },
   "keywords": [],
   "author": "",
   "license": "MIT",
   "devDependencies": {
     "webpack": "^5.38.1",
     "webpack-cli": "^4.7.2"
   }
 }
```

### Asset Management

Prior to webpack, front-end developers would use tools like grunt and gulp to process these assets and move them from their /src folder into their /dist or /build directory. The same idea was used for JavaScript modules, but tools like webpack will dynamically bundle all dependencies (creating what's known as a dependency graph). This is great because every module now explicitly states its dependencies and we'll avoid bundling modules that aren't in use.

### Code Splitting

#### Dynamic Imports
Dynamic Imports 能具体到某个组件的导入

#### Prefetching/Preloading modules

* prefetch: resource is probably needed for some navigation in the future
A preloaded chunk has medium priority and is instantly downloaded. A prefetched chunk is downloaded while the browser is idle.
```
import(/* webpackPrefetch: true */ './path/to/LoginModal.js');
```
* preload: resource will also be needed during the current navigation
A preloaded chunk starts loading in parallel to the parent chunk. A prefetched chunk starts after the parent chunk finishes loading.
```
import(/* webpackPrefetch: true */ './path/to/LoginModal.js');
```

### Caching
todo

### Authoring Libraries
Generally, authoring lib in umd mode, and we can externalise dependancies with externals option

> #### Warning
>The module property should point to a script that utilizes ES2015 module syntax but no other syntax features that aren't yet supported by browsers or node. This enables webpack to parse the module syntax itself, allowing for lighter bundles via tree shaking if users are only consuming certain parts of the library.

### Environment Variables
profile how to use Environment variables on build process...

### Build Performance

1. Stay Up to Date(webpack、node)

todo

### Content Security Policies
todo

### Development - Vagrant
If you have a more advanced project and use Vagrant to run your development environment in a Virtual Machine, you'll often want to also run webpack in the VM.

todo


### Dependency Management

#### require with expression
A context is created if your request contains expressions, so the exact module is not known on compile time.

#### require.context
You can create your own context with the require.context() function.

### Hot Module Replacement
todo

### Tree Shaking
Tree shaking is a term commonly used in the JavaScript context for dead-code elimination. It relies on the static structure of ES2015 module syntax, i.e. import and export. The name and concept have been popularized by the ES2015 module bundler rollup.

The webpack 2 release came with built-in support for ES2015 modules (alias harmony modules) as well as unused module export detection. The new webpack 4 release expands on this capability with a way to provide hints to the compiler via the "sideEffects" package.json property to denote which files in your project are "pure" and therefore safe to prune if unused.

What we've learned is that in order to take advantage of tree shaking, you must...

* Use ES2015 module syntax (i.e. import and export).
* Ensure no compilers transform your ES2015 module syntax into CommonJS modules (this is the default behavior of the popular Babel preset @babel/preset-env - see the documentation for more details).
* Add a "sideEffects" property to your project's package.json file.
* Use the production mode configuration option to enable various optimizations including minification and tree shaking (side effects optimization is enabled in development mode using the flag value).
* Make sure you set a correct value for devtool as some of them can't be used in production mode.

### Production

#### Setup
using  `weback-merge` to merge common config

#### Specify the Mode
Many libraries like rect will key off the process.env.NODE_ENV variable to determine what should be included in the library. For example, when process.env.NODE_ENV is not set to 'production' some libraries may add additional logging and testing to make debugging easier.

#### Minification
Webpack v4+ will minify your code by default in production mode

#### Source Mapping
Avoid inline-*** and eval-*** use in production as they can increase bundle size and reduce the overall performance.

### Lazy Loading
import()

#### Frameworks
Many frameworks and libraries have their own recommendations on how this should be accomplished within their methodologie

#### Further Reading
[Asynchronous vs Deferred JavaScript](https://bitsofco.de/async-vs-defer/)

### ECMAScript Modules

#### Flagging modules as ESM
By default webpack will automatically detect whether a file is an ESM or a different module system.

Node.js established a way of explicitly setting the module type of files by using a property in the package.json. Setting "type": "module" in a package.json does force all files below this package.json to be ECMAScript Modules. Setting "type": "commonjs" will instead force them to be CommonJS Modules.

```
{
  "type": "module"
}
```
In addition to that, files can set the module type by using .mjs or .cjs extension. .mjs will force them to be ESM, .cjs force them to be CommonJs.

### Shimming
 you may only want to deliver those polyfills to the browsers that need patching (i.e. load them on demand).

### Web Workers
TODO

### Progressive Web Application
TODO

### publicPath
runtime public path setting

__webpack_public_path__ = process.env.ASSET_PATH;

### Asset Modules
Asset Modules allow one to use asset files (fonts, icons, etc) without configuring additional loaders.

### Package exports
It replaces the default implementation that returns main field resp
```
{
  "exports": {
    ".": "./main.js",
    "./sub/path": "./secondary.js",
    "./prefix/": "./directory/",
    "./prefix/deep/": "./other-directory/",
    "./other-prefix/*": "./yet-another/*/*.js"
  }
}
```