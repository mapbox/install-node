# install_node

A bash script to install a version of node.js for a platform of your choosing, without depending on nodejs.org being available.

In addition, nodejs binaries cached by Mapbox have had their signatures and shasums verified before being cached.

## Install and run from S3

The latest version of the `install-node` script is always available at `https://mapbox.s3.amazonaws.com/apps/install-node/latest/run`. Versioned `install-node` scripts are still available at `https://mapbox.s3.amazonaws.com/apps/install-node/v{VERSION}/run`, but the latest version is recommended.

You should install the script from S3 with `curl` or `wget` and run it, specifying parameters as environment variables:

```
$ curl https://mapbox.s3.amazonaws.com/apps/install-node/latest/run | NV=14.15.4 NP=linux-x64 OD=/usr/local sh
```

## Usage

```
$ install_node <version> <platform+arch> <dir> [bin_only=true|false]
```

Please note this script uses nodejs compiled binaries, so will not work on Linux distributions using alternative `libc` implementations such as Alpine Linux. nodejs should be compiled from source on those systems.

This will find the corresponding node.js version for the requested `platform+arch` (one of `linux-x64` or `darwin-x64` and drop it into the specified `dir`.

If the optional fourth `bin_only` arg is set to `true` then only the node binary will be installed instead of npm and related resources (headers, man pages, etc.)

*Note: the legacy platform (without arch) arguments `linux`, `darwin` are mapped to the following for backwards compatibility.*

legacy platform arg | platform+arch
--- | ---
linux | linux-x64
darwin | darwin-x64

**Mirror URL**

By default `install_node` will download node from a Mapbox S3 mirror of x64 versions of node.

You can point `install_node` add the official node dist endpoint or your own mirror by using the `INSTALL_NODE_URL` env var.

When using a custom nodejs mirror url, please note that the `install_node` script itself does not perform any validation or verification of the download.

```
$ INSTALL_NODE_URL=http://nodejs.org/dist install_node v14.15.4 linux-x64 /usr/local
```

## Allowed Versions

All versions of nodejs cached by Mapbox are available using any version of the `install-node` script. The most current list of LTS and current nodejs releases cached is available at https://github.com/mapbox/install-node/blob/master/cache/v3/node-versions

## Adding new versions

Update the v3 cache file, and on commit, new versions added will be cached by the Travis CI job.

## v3 Changes

- Deprecated Windows support
- Carbon LTS (v8) and later only
- Updated nodejs team package signatures
