# install_node

A bash script to install a version of node.js for a platform of your choosing.

## Usage

```
$ install_node <semver> <platform> <dir>
```

This will resolve a `semver` range you've given it, find the corresponding node.js
version for the requested `platform` (one of `linux`, `darwin`, `win32`), and drop
it into the specified `dir`.

## Caveats

- Installs x64 versions on darwin and linux, and 32-bit on win32
- win32 version is nothing but `node.exe`. Plan accordingly.
