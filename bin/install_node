#!/bin/bash
set -eu

bin_only="false"

if [ -n "${INSTALL_NODE_URL+1}" ]; then
    INSTALL_NODE_URL=$INSTALL_NODE_URL
else
    INSTALL_NODE_URL="https://s3.amazonaws.com/mapbox/vendor/nodejs"
fi

if [ -z "${NV+1}" -o -z "${NP+1}" -o -z "${OD+1}" ]; then
    node_version=$1
    platformarch=$2
    output_dir=$3
    if [ -n "${4+1}" ]; then
        bin_only=$4
    fi
else
    node_version=$NV
    platformarch=$NP
    output_dir=$OD
    if [ -n "${BO+1}" ]; then
        bin_only=$BO
    fi
fi

# Enforce default arch for legacy arch-free args.
case $platformarch in
    "linux")
        platformarch="linux-x64"
        ;;
    "darwin")
        platformarch="darwin-x64"
        ;;
esac

# Output info about requested range and resolved node version
echo "Requested node version: $node_version"
echo "Requested node platform+arch: $platformarch"

if [ "$bin_only" = "true" ]; then
    echo "Requested bin only: $bin_only"
fi

majorv="$(echo $node_version | cut -c 1)"
case $platformarch in
    "linux-x64")
        url="$INSTALL_NODE_URL/v$node_version/node-v$node_version-linux-x64.tar.gz"
        ;;
    "darwin-x64")
        url="$INSTALL_NODE_URL/v$node_version/node-v$node_version-darwin-x64.tar.gz"
        ;;
    *)
        echo "Not a valid platform+arch. Specify one of linux-x64|darwin-x64"
        exit 1
        ;;
esac

tmp="$(mktemp -d -t install-node.XXXXXX)"
trap "rm -rf $tmp" EXIT

cd "$tmp"

# Download node
if type curl > /dev/null; then
    curl -Lsfo $(basename $url) "$url"
elif type wget > /dev/null; then
    wget -qO $(basename $url) "$url"
else
    echo "Either curl or wget required for install-node"
    exit 1
fi

# Install node
if [ "$bin_only" = "true" ]; then
    tar --strip 2 -xzf "node-v$node_version-$platformarch.tar.gz" --directory "$output_dir" "node-v$node_version-$platformarch/bin/node"
else
    tar --strip-components 1 -xzf "node-v$node_version-$platformarch.tar.gz" --no-same-owner --directory "$output_dir"
    # Change the location of the npm cache so different versions of node don't interfere with each other
    mkdir -p "$output_dir/.npm"
    "$output_dir/lib/node_modules/npm/configure" --cache="$output_dir/.npm"
fi
