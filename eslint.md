# Typical usage

```

```

# Core Concepts

## Plugins

An ESLint plugin is an npm module that can contain a set of ESLint rules, configurations, processors, and languages. Often plugins include custom rules. Plugins can be used to enforce a style guide and support JavaScript extensions (like TypeScript), libraries (like React), and frameworks (Angular).

## Parsers

An ESLint parser converts code into an abstract syntax tree that ESLint can evaluate. By default, ESLint uses the built-in Espree parser, which is compatible with standard JavaScript runtimes and versions.

Custom parsers let ESLint parse non-standard JavaScript syntax. Often custom parsers are included as part of shareable configurations or plugins, so you don’t have to use them directly.

For example, @typescript-eslint/parser is a custom parser included in the typescript-eslint project that lets ESLint parse TypeScript code.

## Custom Processors

An ESLint processor extracts JavaScript code from other kinds of files, then lets ESLint lint the JavaScript code. Alternatively, you can use a processor to manipulate JavaScript code before parsing it with ESLint.

For example, @eslint/markdown contains a custom processor that lets you lint JavaScript code inside of Markdown code blocks.

# Configure ESLint

## Configuration Files

### Configuration Objects

- name - A name for the configuration object. This is used in error messages and config inspector to help identify which configuration object is being used. (Naming Convention)

- files - An array of glob patterns indicating the files that the configuration object should apply to. If not specified, the configuration object applies to all files matched by any other configuration object.

* ignores - An array of glob patterns indicating the files that the configuration object should not apply to. If not specified, the configuration object applies to all files matched by files. If ignores is used without any other keys in the configuration object, then the patterns act as global ignores.

* languageOptions - An object containing settings related to how JavaScript is configured for linting.
  - ecmaVersion - The version of ECMAScript to support. May be any year (i.e., 2022) or version (i.e., 5). Set to "latest" for the most recent supported version. (default: "latest")
  - sourceType - The type of JavaScript source code. Possible values are "script" for traditional script files, "module" for ECMAScript modules (ESM), and "commonjs" for CommonJS files. (default: "module" for .js and .mjs files; "commonjs" for .cjs files)
  - globals - An object specifying additional objects that should be added to the global scope during linting.
  - parser - An object containing a parse() method or a parseForESLint() method. (default: espree)
  - parserOptions - An object specifying additional options that are passed directly to the parse() or parseForESLint() method on the parser. The available options are parser-dependent.
  * processor - Either an object containing preprocess() and postprocess() methods or a string indicating the name of a processor inside of a plugin (i.e., "pluginName/processorName").
  * plugins - An object containing a name-value mapping of plugin names to plugin objects. When files is specified, these plugins are only available to the matching files.
  * rules - An object containing the configured rules. When files or ignores are specified, these rule configurations are only available to the matching files.
  * settings - An object containing name-value pairs of information that should be available to all rules.

### Configuration File Resolution

When ESLint is run on the command line, it first checks the current working directory for eslint.config.js. If that file is found, then the search stops, otherwise it checks for eslint.config.mjs. If that file is found, then the search stops, otherwise it checks for eslint.config.cjs. If none of the files are found, it checks the parent directory for each file. This search continues until either a config file is found or the root directory is reached.

You can prevent this search for eslint.config.js by using the -c or --config option on the command line to specify an alternate configuration file
