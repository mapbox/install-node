#!/usr/bin/env bash

install_node=$(dirname $0)/../bin/install_node
failed="0"

function assertNo() {
    if [ -e $1 ]; then
        echo "not ok - notfound $1";
        failed="1"
    else
        echo "ok - notfound $1"
    fi
}

function assertOk() {
    if [ -e $1 ]; then
        echo "ok - found $1"
    else
        echo "not ok - found $1";
        failed="1"
    fi
}

function assertMD5() {
    if [ -e $1 ]; then
        echo "ok - found $1 $(md5sum $1 | grep -oE '[0-f]{32}')"
    else
        echo "not ok - found $1";
        failed="1"
    fi
}

function assertExit0() {
    if $1 &> /dev/null; then
        echo "ok - exit 0 $1";
    else
        echo "not ok - exit 0 $1";
        failed="1"
    fi
}

function assertExit1() {
    if $1 &> /dev/null; then
        echo "not ok - exit 1+ $1";
        failed="1"
    else
        echo "ok - exit 1+ $1";
    fi
}

function failure() {
    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# install_node $1 $2 $testdir"
    assertExit1 "$install_node $1 $2 $testdir"
    assertNo $testdir/bin/node
    assertNo $testdir/bin/npm
    assertNo $testdir/include/node
    assertNo $testdir/lib/node_modules/npm
    assertNo $testdir/share/man
    assertNo $testdir/.npm
    rm -rf $testdir

    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# NV=$1 NP=$2 OD=$testdir install_node"
    NV=$1 NP=$2 OD=$testdir assertExit1 "$install_node"
    assertNo $testdir/bin/node
    assertNo $testdir/bin/npm
    assertNo $testdir/include/node
    assertNo $testdir/lib/node_modules/npm
    assertNo $testdir/share/man
    assertNo $testdir/.npm
    rm -rf $testdir
}

function successUnix() {
    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# install_node $1 $2 $testdir"
    assertExit0 "$install_node $1 $2 $testdir"
    assertMD5 $testdir/bin/node
    assertOk $testdir/bin/npm
    assertOk $testdir/include/node
    assertOk $testdir/lib/node_modules/npm
    assertOk $testdir/share/man
    assertOk $testdir/.npm
    rm -rf $testdir

    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# NV=$1 NP=$2 OD=$testdir install_node"
    NV=$1 NP=$2 OD=$testdir assertExit0 "$install_node"
    assertMD5 $testdir/bin/node
    assertOk $testdir/bin/npm
    assertOk $testdir/include/node
    assertOk $testdir/lib/node_modules/npm
    assertOk $testdir/share/man
    assertOk $testdir/.npm
    rm -rf $testdir
}

function successUnixBin() {
    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# install_node $1 $2 $testdir true"
    assertExit0 "$install_node $1 $2 $testdir true"
    assertMD5 $testdir/node
    assertNo $testdir/bin
    assertNo $testdir/include
    assertNo $testdir/lib
    assertNo $testdir/share
    assertNo $testdir/.npm
    rm -rf $testdir

    local testdir=$(mktemp -d -t test-install-node.XXXXXX)
    echo "# NV=$1 NP=$2 OD=$testdir BO=true install_node"
    NV=$1 NP=$2 OD=$testdir BO=true assertExit0 "$install_node"
    assertMD5 $testdir/node
    assertNo $testdir/bin
    assertNo $testdir/include
    assertNo $testdir/lib
    assertNo $testdir/share
    assertNo $testdir/.npm
    rm -rf $testdir
}

# official dist
INSTALL_NODE_URL=http://nodejs.org/dist successUnix 10.15.3 linux
# normal s3 mirror
successUnix 10.15.3 linux
successUnix 10.15.3 linux-x64
successUnix 10.15.3 darwin
successUnix 10.15.3 darwin-x64
# invalid version
failure foo.bar linux
# invalid platform
failure 0.10.30 freebsd

if [ "$failed" == "1" ]; then
    exit 1
else
    exit 0
fi
