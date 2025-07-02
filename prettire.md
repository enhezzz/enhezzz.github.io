# Prettier

## about

Prettier enforces a consistent code style (i.e. code formatting that won’t affect the AST) across your entire codebase because it disregards the original styling\* by parsing it away and re-printing the parsed AST with its own rules that take the maximum line length into account, wrapping code when necessary.

## Prettier vs. Linters

Linters have two categories of rules:

- Formatting rules
- Code-quality rules

In other words, use Prettier for formatting and linters for catching bugs!

## Usage

### Set up your editor

[ref](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

### ESLint (and other linters)

If you use ESLint, install [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier#installation) to make ESLint and Prettier play nice with each other. It turns off all ESLint rules that are unnecessary or might conflict with Prettier. There’s a similar config for Stylelint: [stylelint-config-prettier](https://github.com/prettier/stylelint-config-prettier)

### Git hooks

1. Install husky and lint-staged:

```
npm install --save-dev husky lint-staged
npx husky init
node --eval "fs.writeFileSync('.husky/pre-commit','npx lint-staged\n')"
```

2. Add the following to your package.json:

```
{
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
}
```

> Note: If you use ESLint, make sure lint-staged runs it before Prettier, not after.

### Summary

To summarize, we have learned to:

- Install an exact version of Prettier locally in your project. This makes sure that everyone in the project gets the exact same version of Prettier. Even a patch release of Prettier can result in slightly different formatting, so you wouldn’t want different team members using different versions and formatting each other’s changes back and forth.
- Add a .prettierrc to let your editor know that you are using Prettier.
- Add a .prettierignore to let your editor know which files not to touch, as well as for being able to run prettier --write . to format the entire project (without mangling files you don’t want, or choking on generated files).
- Run prettier --check . in CI to make sure that your project stays formatted.
- Run Prettier from your editor for the best experience.
- Use eslint-config-prettier to make Prettier and ESLint play nice together.
- Set up a pre-commit hook to make sure that every commit is formatted.

### Plugins

Plugins are ways of adding new languages or formatting rules to Prettier. Prettier’s own implementations of all languages are expressed using the plugin API. The core prettier package contains JavaScript and other web-focused languages built in. For additional languages you’ll need to install a plugin.

## Configuring Prettier

### Options

change on demand. Generally no need to change

### Configuration File

Prettier intentionally doesn’t support any kind of global configuration

#### Configuration Overrides

Overrides let you have different configuration for certain file extensions, folders and specific files.

#### EditorConfig

If a .editorconfig file is in your project, Prettier will parse it and convert its properties to the corresponding Prettier configuration. This configuration will be overridden by .prettierrc, etc.

## Editor Integration

[prettier-vscode](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
