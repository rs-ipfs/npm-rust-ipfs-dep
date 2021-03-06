# rust-ipfs-dep

> Download [rust-ipfs](https://github.com/ipfs-rust/rust-ipfs/) to your node_modules.

[![](https://img.shields.io/badge/project-IPFS-blue.svg?style=flat-square)](http://ipfs.io/)
[![Back on OpenCollective](https://img.shields.io/badge/open%20collective-donate-yellow.svg)](https://opencollective.com/ipfs-rust) [![Matrix](https://img.shields.io/badge/matrix-%23rust_ipfs%3Amatrix.org-blue.svg)](https://riot.im/app/#/room/#rust-ipfs:matrix.org) [![Discord](https://img.shields.io/discord/475789330380488707?color=blueviolet&label=discord)](https://discord.gg/9E5SFvW) 

# Project Status - `Pre-Alpha`

This repository is for testing purposes only and as such has not been published to NPM yet. Look for more updates as time goes on

# Installation

```
# Coming soon!
npm install rust-ipfs-dep --save
```

See [IPFS getting-started](http://ipfs.io/docs/getting-started). If anything goes wrong, try using: [http://ipfs.io/docs/install](http://ipfs.io/docs/install).

## Usage

This module downloads `rust-ipfs` binaries from https://github.com/ipfs-rust/rust-ipfs/releases into your project.

By default it will download the rust-ipfs version that matches the npm version of this module. So depending on `rust-ipfs-dep@0.1.0` will install `rust-ipfs v0.1.0` for your current system architecture, in to your project at `node_modules/rust-ipfs-dep/ipfs`.

After downloading you can find out the path of the installed binary by calling the `path` function exported by this module:

```javascript
const { path } = require('rust-ipfs-dep')

console.info('rust-ipfs is installed at', path())
```

An error will be thrown if the path to the binary cannot be resolved - if you do not wish this to happen, call `path.silent()`:

```javascript
const { path: silent } = require('rust-ipfs-dep')

console.info('rust-ipfs may be installed at', silent())
```

### Overriding the rust-ipfs version

You can override the version of rust-ipfs that gets downloaded by adding by adding a `rust-ipfs.version` field to your `package.json`

```json
"rust-ipfs": {
  "version": "v0.4.13"
},
```

### Using local IPFS daemon as the package download url

The url to download the binaries from can be specified by adding a field `rust-ipfs.distUrl` field to your `package.json`, eg:

```json
"rust-ipfs": {
  "version": "v0.4.3",
  "distUrl": "http://localhost:8080/ipfs/QmSoNtqW22htkg9mtHWNBvZLUEmqfq8su7957meS1iQfeL"
},
```

Where `QmSoNtqW22htkg9mtHWNBvZLUEmqfq8su7957meS1iQfeL` is the root of the distributions web site.

Or when run with `node src/bin.js`, the dist url can be passed via an environment variable `RUST_IPFS_DIST_URL`, eg:

```
RUST_IPFS_DIST_URL=http://localhost:8080/ipfs/QmSoNtqW22htkg9mtHWNBvZLUEmqfq8su7957meS1iQfeL node binsrc/bin.js
```

### Arguments

When used via `node src/bin.js`, you can specify the target platform, version and architecture via environment variables: `TARGET_OS`, `TARGET_VERSION` and `TARGET_ARCH`.

We fetch the versions dynamically from `https://github.com/ipfs-rust/rust-ipfs/releases`.

Or via command line arguments in the order of:

```
node src/bin.js <version> <platform> <architecture> <install directory>
```

```
node src/bin.js v0.4.3 linux amd64 ./rust-ipfs
```

## Development

**Note**: The binary gets put in the `rust-ipfs` folder inside the module folder.

## Deployment

Publishing is handled by GitHub Actions. The workflow is set to run every hour. It checks dist.ipfs.io for the latest rust-ipfs version number, and compares with the version property in the package.json for this module. If they are different, the action will update the version of this module, publish it to npm, and push the change back to the master branch.

- `.github/main.workflow` defines how frequently we check for a new rust-ipfs release, (hourly) and the steps to carry out.
- `action-check-for-rust-ipfs-release` compares the module version with the latest rust-ipfs release as published to dist.ipfs.io. This lets us bail early if everything is up to date.
- `action-publish` handles updating the module version, publishing to npm, and pushing the changes back to the repo.

If for some reason you need to run it manually, follow the instructions below.

### Publish a Prerelease to npm

You have made changes and want to triple check everything is working. You can! If you publish with a numeric prerelease identifier then `rust-ipfs-dep` will strip it and install the corresponding version e.g. `0.4.19-0` installs `rust-ipfs` version `0.4.19`.

To deploy a new version with a prerelease identifier run the following command:

```sh
npx aegir release --type prepatch --preid '' --dist-tag next --no-lint --no-test --no-build --no-docs
# Note: change "--type prepatch" to the appropriate prerelease type.
# e.g. prepatch: 0.4.18 => 0.4.19-0, preminor: 0.4.18 => 0.5.0-0 etc.

# Increment prerelease (e.g. 0.4.19-0 -> 0.4.19-1)
npx aegir release --type prerelease --preid '' --dist-tag next --no-lint --no-test --no-build --no-docs
```

This publishes to the "next" tag meaning that the current "latest" version of `rust-ipfs-dep` will remain the same.

### Publish a Release to npm

When you're finally ready to release:

```sh
npx aegir release --type=patch --no-lint --no-test --no-build --no-docs
```
