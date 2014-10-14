# install_node

A bash script to install a version of node.js for a platform of your choosing,
without depending on nodejs.org being available.

## Usage

```
$ install_node <version> <platform> <dir> [bin_only=true|false]
```

This will find the corresponding node.js version for the requested `platform`
(one of `linux`, `darwin`, `win32`), and drop it into the specified `dir`.

If the optional fourth `bin_only` arg is set to `true` then only the node binary
will be installed instead of npm and related resources (headers, man pages, etc.)

## Run from S3

Alternately you can pull the script from S3 with `curl` and run it, specifying
parameters as environment variables:

```
$ curl https://s3.amazonaws.com/mapbox/apps/install-node/v0.0.3/run | NV=0.10.26 NP=linux OD=/usr/local sh
```

## Allowed Versions

See [`cache/node-versions`](https://github.com/mapbox/install-node/blob/master/cache/node-versions).

## Caveats

- Installs x64 versions on darwin and linux, and 32-bit on win32
- win32 version is nothing but `node.exe`. Plan accordingly.
