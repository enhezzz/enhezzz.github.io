## Motivation

## install

1.  Node.js (at least v18.12)
    npm install -g pnpm@latest-10

2.  packaged with Node.js into an executable
    npm install -g @pnpm/exe@latest-10

## usage

### Configuring

- compatible with any config from npm
- set location of global store

```
pnpm config set store-dir /path/to/.pnpm-store
```

If no store is configured, then pnpm will automatically create a store on the same drive. If you need pnpm to work across multiple hard drives or filesystems, see [faq](https://pnpm.io/faq#does-pnpm-work-across-multiple-drives-or-filesystems)

### cli

| npm command   | pnpm equivalent |
| ------------- | --------------- |
| npm install   | pnpm install    |
| npm i <pkg>   | pnpm add <pkg>  |
| npm run <cmd> | pnpm <cmd>      |

### filtering

Filtering allows you to restrict commands to specific subsets of packages.

## CLI commands

### Manage dependencies

#### pnpm add

- Install from npm registry
- Install from the workspace
- Install from local file system(same as npm link)

#### pnpm install

- pnpm install is used to install all dependencies for a project.
- Inside a workspace, pnpm install installs all dependencies in all the projects.

#### pnpm update

pnpm update updates packages to their latest version based on the specified range.

#### pnpm link

Makes the current local package accessible system-wide, or in another location.

##### pnpm link <dir>

Links package from <dir> directory to node_modules of package from where you're executing this command.

##### pnpm link

Links package from location where this command was executed to global node_modules, so it can be referred from another package with pnpm link <pkg>. Also if the package has a bin field, then the package's binaries become available system-wide.

##### pnpm link <pkg>

Links the specified package (<pkg>) from global node_modules to the node_modules of package from where this command was executed.

##### Add a binary globally

Remember that the binary will be available only if the package has a bin field in its package.json.

```shell
cd ~/projects/foo
pnpm install # install dependencies of foo
pnpm link # link foo globally
```

##### difference between pnpm link and using the file: protocol

| Feature                                      | pnpm link                               | file: Protocol                                     |
| -------------------------------------------- | --------------------------------------- | -------------------------------------------------- |
| Symlink/Hard-link                            | Symlink                                 | Hard-link                                          |
| Reflects source code modifications           | Yes                                     | Yes                                                |
| Installs dependencies of the linked package  | No (manual installation required)       | Yes (overrides node_modules of the linked package) |
| Use different package manager for dependency | Possible (e.g., use npm for linked pkg) | No, it will use pnpm                               |

#### pnpm import

pnpm import generates a pnpm-lock.yaml from another package manager's lockfile.

#### pnpm dedupe

Perform an install removing older dependencies in the lockfile if a newer version can be used.

- --check
  Check if running dedupe would result in changes without installing packages or editing the lockfile. Exits with a non-zero status code if changes are possible.

### Patch dependencies

TODO

### Review dependencies
