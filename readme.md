# install_node

A bash script to install a version of node.js for a platform of your choosing,
without depending on nodejs.org being available.

## Usage

```
$ install_node <version> <platform+arch> <dir> [bin_only=true|false]
```

This will find the corresponding node.js version for the requested `platform+arch`
(one of `linux-x64`, `darwin-x64`, `win32-ia32`, `win32-x64`), and drop it into the specified `dir`.

If the optional fourth `bin_only` arg is set to `true` then only the node binary
will be installed instead of npm and related resources (headers, man pages, etc.)

*Note: the legacy platform (without arch) arguments `linux`, `darwin`, `win32` are
mapped to the following for backwards compatibility.*

legacy platform arg | platform+arch
--- | ---
linux | linux-x64
darwin | darwin-x64
win32 | win32-ia32

## Run from S3

Alternately you can pull the script from S3 with `curl` and run it, specifying
parameters as environment variables:

```
$ curl https://s3.amazonaws.com/mapbox/apps/install-node/v0.1.2/run | NV=0.10.26 NP=linux-x64 OD=/usr/local sh
```

## Allowed Versions

See [`cache/node-versions`](https://github.com/mapbox/install-node/blob/master/cache/node-versions).
To add a newly released node version to the mirror add it to this file and commit
-- it will be cached automatically by travis.

## Caveats

- win32 version is nothing but `node.exe`. Plan accordingly.
