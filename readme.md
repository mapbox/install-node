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

**Mirror URL**

By default `install_node` will download node from a Mapbox S3 mirror of x64 versions of node.

You can point `install_node` add the official node dist endpoint or your own mirror by using the `INSTALL_NODE_URL` env var.

```
$ INSTALL_NODE_URL=http://nodejs.org/dist install_node v0.10.33 linux-x64 /usr/local
```

## Run from S3

Alternately you can pull the script from S3 with `curl` and run it, specifying
parameters as environment variables:

```
$ curl https://s3.amazonaws.com/mapbox/apps/install-node/v2.0.0/run | NV=4.4.2 NP=linux-x64 OD=/usr/local sh
```

## Allowed Versions

Look for a file `node-versions` in the [`cache/v#/node-versions`](https://github.com/mapbox/install-node/blob/master/cache). The number after `v#/` corresponds with the major version number that install-node is currently on (found in package.json).

To add a newly released node version to the mirror add it to the most recent `node-versions` file and commit
-- it will be cached automatically by travis.

If you need to update `install_node` to support a new release of node that isn't currently supported, you will need to bump the major version, start a new `v#/node-versions` list with the new node version and update buildpack.

## Caveats

- win32 version is nothing but `node.exe`. Plan accordingly.
