<<<<<<<+4.x
<p align="center">
  <img src="assets/logo/web3js.jpg" width="300" alt="web3.js" />
=======
<p style="text-align: center;">
  <img src="assets/logo/web3js.jpg" width="200" alt="web3.js">
>>>>>>>+origin/1.x
</p>

# Web3.js

<<<<<<<+4.x
[![Dependency Status][downloads-image]][npm-url] ![Unit Test Coverage](https://img.shields.io/codecov/c/github/web3/web3.js/4.x?label=unit%20test%20coverage)
![Commit Activity](https://img.shields.io/github/commit-activity/m/web3/web3.js/4.x?label=commit%20activity%20on%204.x)
![Contributors](https://img.shields.io/github/contributors/web3/web3.js?label=contributors%20on%20all%20branches)
=======
[![NPM Package Downloads][npm-image-downloads]][npm-url] [![cdnhits][cdnhits-image]][cdnhits-url] [![Discord][discord-image]][discord-url] [![StackExchange][stackexchange-image]][stackexchange-url] [![NPM Package Version][npm-image-version]][npm-url] [![Build Status][actions-image]][actions-url]  [![Coverage Status][coveralls-image]][coveralls-url] [![Lerna][lerna-image]][lerna-url] [![Netlify Status][netlify-image]][netlify-url] [![GitPOAP Badge][gitpoap-image]][gitpoap-url] [![Twitter][twitter-image]][twitter-url]

#####  [Web3.js 4.x][4x-release] has been released. Checkout 4.x [API documentation and migration guide][4xdoc] for testing, early feedback and contributions. 
>>>>>>>+origin/1.x

![ES Version](https://img.shields.io/badge/ES-2020-yellow)
![Node Version](https://img.shields.io/badge/node-14.x-green)

Web3.js is a TypeScript implementation of the [Ethereum JSON RPC API](https://eth.wiki/json-rpc/API) and related tooling maintained by [ChainSafe Systems](https://chainsafe.io).

## Installation

You can install the package either using [NPM](https://www.npmjs.com/package/web3) or using [Yarn](https://yarnpkg.com/package/web3)
<<<<<<<+4.x

> If you wanna checkout latest bugfix or feature, use `npm install web3@dev`
=======

### Using NPM

```bash
npm install web3
```

### Yarn

```bash
yarn add web3
```

### In the Browser

Use the prebuilt `dist/web3.min.js`, or
build using the [web3.js][repo] repository:

```bash
npm run build
```

Then include `dist/web3.min.js` in your html file.
This will expose `Web3` on the window object.

Or via jsDelivr CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
```

UNPKG:

```html
<script src="https://unpkg.com/web3@latest/dist/web3.min.js"></script>
```

## Usage

```js
// In Node.js
const Web3 = require('web3');
const web3 = new Web3('ws://localhost:8546');
console.log(web3);
// Output
{
    eth: ... ,
    shh: ... ,
    utils: ...,
    ...
}
```

Additionally you can set a provider using `web3.setProvider()` (e.g. WebsocketProvider):

```js
web3.setProvider('ws://localhost:8546');
// or
web3.setProvider(new Web3.providers.WebsocketProvider('ws://localhost:8546'));
```

There you go, now you can use it:

```js
web3.eth.getAccounts().then(console.log);
```

### Usage with TypeScript

We support types within the repo itself. Please open an issue here if you find any wrong types.

You can use `web3.js` as follows:

```typescript
import Web3 from 'web3';
import { BlockHeader, Block } from 'web3-eth' // ex. package types
const web3 = new Web3('ws://localhost:8546');
```

If you are using the types in a `commonjs` module, like in a Node app, you just have to enable `esModuleInterop` and `allowSyntheticDefaultImports` in your `tsconfig` for typesystem compatibility:

```js
"compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    ....
```

## Troubleshooting and known issues.

### Web3 and Create-react-app

**1.8 UPDATE: If you are facing any issues with create-react-app or angular, make sure you are using a web3 version of 1.8.0 or greater, as its been fixed** 

If you are using create-react-app version >=5 you may run into issues building. This is because NodeJS polyfills are not included in the latest version of create-react-app. 

### Solution


- Install react-app-rewired and the missing modules

If you are using yarn:
```bash
yarn add --dev react-app-rewired process crypto-browserify stream-browserify assert stream-http https-browserify os-browserify url buffer
```

If you are using npm:
```bash
npm install --save-dev react-app-rewired crypto-browserify stream-browserify assert stream-http https-browserify os-browserify url buffer process
```

- Create `config-overrides.js` in the root of your project folder with the content:

```javascript
const webpack = require('webpack');

module.exports = function override(config) {
    const fallback = config.resolve.fallback || {};
    Object.assign(fallback, {
        "crypto": require.resolve("crypto-browserify"),
        "stream": require.resolve("stream-browserify"),
        "assert": require.resolve("assert"),
        "http": require.resolve("stream-http"),
        "https": require.resolve("https-browserify"),
        "os": require.resolve("os-browserify"),
        "url": require.resolve("url")
    })
    config.resolve.fallback = fallback;
    config.plugins = (config.plugins || []).concat([
        new webpack.ProvidePlugin({
            process: 'process/browser',
            Buffer: ['buffer', 'Buffer']
        })
    ])
    return config;
}
```

- Within `package.json` change the scripts field for start, build and test. Instead of `react-scripts` replace it with `react-app-rewired`

before: 
```typescript
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
},
```

after:
```typescript
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
},
```

The missing Nodejs polyfills should be included now and your app should be functional with web3.
- If you want to hide the warnings created by the console:

In `config-overrides.js` within the `override` function, add:

```javascript
config.ignoreWarnings = [/Failed to parse source map/];
```

### Web3 and Angular

### New solution

If you are using Angular version >11 and run into an issue building, the old solution below will not work. This is because polyfills are not included in the newest version of Angular.

- Install the required dependencies within your angular project:

```bash
npm install --save-dev crypto-browserify stream-browserify assert stream-http https-browserify os-browserify
```

- Within `tsconfig.json` add the following `paths` in `compilerOptions` so Webpack can get the correct dependencies

```typescript
{
    "compilerOptions": {
        "paths" : {
        "crypto": ["./node_modules/crypto-browserify"],
        "stream": ["./node_modules/stream-browserify"],
        "assert": ["./node_modules/assert"],
        "http": ["./node_modules/stream-http"],
        "https": ["./node_modules/https-browserify"],
        "os": ["./node_modules/os-browserify"],
    }
}
```

- Add the following lines to `polyfills.ts` file:

```typescript
import { Buffer } from 'buffer';

(window as any).global = window;
global.Buffer = Buffer;
global.process = {
    env: { DEBUG: undefined },
    version: '',
    nextTick: require('next-tick')
} as any;
```

### Old solution

If you are using Ionic/Angular at a version >5 you may run into a build error in which modules `crypto` and `stream` are `undefined`

a workaround for this is to go into your node-modules and at `/angular-cli-files/models/webpack-configs/browser.js` change  the `node: false` to `node: {crypto: true, stream: true}` as mentioned [here](https://github.com/ethereum/web3.js/issues/2260#issuecomment-458519127)

Another variation of this problem was an [issue opned on angular-cli](https://github.com/angular/angular-cli/issues/1548)

## Documentation

Documentation can be found at [ReadTheDocs][docs].
>>>>>>>+origin/1.x

### Using NPM

```bash
npm install web3
```

### Using Yarn

```bash
yarn add web3
```

<<<<<<<+4.x
## Getting Started

-   :writing_hand: If you have questions [submit an issue](https://github.com/ChainSafe/web3.js/issues/new/choose) or join us on [Discord](https://discord.gg/yjyvFRP)
    ![Discord](https://img.shields.io/discord/593655374469660673.svg?label=Discord&logo=discord)

## Prerequisites

-   :gear: [NodeJS](https://nodejs.org/) (LTS/Fermium)
-   :toolbox: [Yarn](https://yarnpkg.com/)/[Lerna](https://lerna.js.org/)

## Migration Guide

-   [Migration Guide from Web3.js 1.x to 4.x](https://docs.web3js.org/guides/web3_upgrade_guide/x/)
    Breaking changes are listed in migration guide and its first step for migrating from Web3.js 1.x to 4.x. If there is any question or discussion feel free to ask in [Discord](https://discord.gg/yjyvFRP), and in case of any bug or new feature request [open issue](https://github.com/web3/web3.js/issues/new) or create a pull request for [contributions](https://github.com/web3/web3.js/blob/4.x/CONTRIBUTIONS.md).

## Useful links

-   [Web3 tree shaking support guide](https://docs.web3js.org/guides/advanced/web3_tree_shaking_support_guide/)
-   [React App Example](https://github.com/ChainSafe/web3js-example-react-app)

## Architecture Overview

| Package                                                                                           | Version                                                                                                                                                                           | License                                                                                                               | Docs                                                                                                           | Description                                                                                                   |
| ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [web3](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3)                               | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3%2Fpackage.json)](https://www.npmjs.com/package/web3)                               | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3)                | :rotating_light: Entire Web3.js offering (includes all packages)                                              |
| [web3-core](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-core)                     | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-core%2Fpackage.json)](https://www.npmjs.com/package/web3-core)                     | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-core)           | Core functions for web3.js packages                                                                           |
| [web3-errors](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-errors)                 | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-errors%2Fpackage.json)](https://www.npmjs.com/package/web3-core)                   | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-errors)         | Errors Objects                                                                                                |
| [web3-eth](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth)                       | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth%2Fpackage.json)](https://www.npmjs.com/package/web3-eth)                       | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth)            | Modules to interact with the Ethereum blockchain and smart contracts                                          |
| [web3-eth-abi](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-abi)               | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-abi%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-abi)               | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-abi)        | Functions for encoding and decoding EVM in/output                                                             |
| [web3-eth-accounts](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-accounts)     | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-accounts%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-accounts)     | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-accounts)   | Functions for managing Ethereum accounts and signing                                                          |
| [web3-eth-contract](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-contract)     | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-contract%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-contract)     | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-contract)   | The contract package contained in [web3-eth](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth) |
| [web3-eth-ens](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-ens)               | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-ens%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-ens)               | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-ens)        | Functions for interacting with the Ethereum Name Service                                                      |
| [web3-eth-iban](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-iban)             | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-iban%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-iban)             | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-iban)       | Functionality for converting Ethereum addressed to IBAN addressed and vice versa                              |
| [web3-eth-personal](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-eth-personal)     | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-eth-personal%2Fpackage.json)](https://www.npmjs.com/package/web3-eth-personal)     | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-eth-personal)   | Module to interact with the Ethereum blockchain accounts stored in the node                                   |
| [web3-net](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-net)                       | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-net%2Fpackage.json)](https://www.npmjs.com/package/web3-net)                       | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-net)            | Functions to interact with an Ethereum node's network properties                                              |
| [web3-providers-http](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-providers-http) | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-providers-http%2Fpackage.json)](https://www.npmjs.com/package/web3-providers-http) | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-providers-http) | Web3.js provider for the HTTP protocol                                                                        |
| [web3-providers-ipc](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-providers-ipc)   | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-providers-ipc%2Fpackage.json)](https://www.npmjs.com/package/web3-providers-ipc)   | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-providers-ipc)  | Web3.js provider for IPC                                                                                      |
| [web3-providers-ws](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-providers-ws)     | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-providers-ws%2Fpackage.json)](https://www.npmjs.com/package/web3-providers-ws)     | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-providers-ws)   | Web3.js provider for the Websocket protocol                                                                   |
| [web3-rpc-methods](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-rpc-methods)       | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-rpc-methods%2Fpackage.json)](https://www.npmjs.com/package/web3-types)             | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/)                    | RPC Methods                                                                                                   |
| [web3-types](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-types)                   | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-types%2Fpackage.json)](https://www.npmjs.com/package/web3-types)                   | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-types)          | Shared useable types                                                                                          |
| [web3-utils](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-utils)                   | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-utils%2Fpackage.json)](https://www.npmjs.com/package/web3-utils)                   | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-utils)          | Useful utility functions for Dapp developers                                                                  |
| [web3-validator](https://github.com/ChainSafe/web3.js/tree/4.x/packages/web3-validator)           | [![npm](https://img.shields.io/github/package-json/v/web3/web3.js/4.x?filename=packages%2Fweb3-validator%2Fpackage.json)](https://www.npmjs.com/package/web3-validator)           | [![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0) | [![documentation](https://img.shields.io/badge/typedoc-blue)](https://docs.web3js.org/api/web3-validator)      | Utilities for validating objects                                                                              |

## Package.json Scripts

| Script           | Description                                                        |
| ---------------- | ------------------------------------------------------------------ |
| clean            | Uses `rimraf` to remove `dist/`                                    |
| build            | Uses `tsc` to build all packages                                   |
| lint             | Uses `eslint` to lint all packages                                 |
| lint:fix         | Uses `eslint` to check and fix any warnings                        |
| format           | Uses `prettier` to format the code                                 |
| test             | Uses `jest` to run unit tests in each package                      |
| test:integration | Uses `jest` to run tests under `/test/integration` in each package |
| test:unit        | Uses `jest` to run tests under `/test/unit` in each package        |
=======
### Contributing

Please follow the [Contribution Guidelines](./CONTRIBUTIONS.md) and [Review Guidelines](./REVIEW.md).

This project adheres to the [Release Guidelines](./REVIEW.md).

### Community

-   [Discord][discord-url]
-   [StackExchange][stackexchange-url]

### Similar libraries in other languages

-   Haskell: [hs-web3](https://github.com/airalab/hs-web3)
-   Java: [web3j](https://github.com/web3j/web3j)
-   PHP: [web3.php](https://github.com/web3p/web3.php)
-   Purescript: [purescript-web3](https://github.com/f-o-a-m/purescript-web3)
-   Python: [Web3.py](https://github.com/ethereum/web3.py)
-   Ruby: [ethereum.rb](https://github.com/EthWorks/ethereum.rb)
-   Scala: [web3j-scala](https://github.com/mslinn/web3j-scala)
>>>>>>>+origin/1.x

[npm-url]: https://npmjs.org/package/web3
<<<<<<<+4.x
[downloads-image]: https://img.shields.io/npm/dm/web3?label=npm%20downloads
=======
[actions-image]: https://github.com/ethereum/web3.js/workflows/Build/badge.svg
[actions-url]: https://github.com/ethereum/web3.js/actions
[coveralls-image]: https://coveralls.io/repos/ethereum/web3.js/badge.svg?branch=1.x
[coveralls-url]: https://coveralls.io/r/ethereum/web3.js?branch=1.x
[waffle-image]: https://badge.waffle.io/ethereum/web3.js.svg?label=ready&title=Ready
[waffle-url]: https://waffle.io/ethereum/web3.js
[discord-image]: https://img.shields.io/discord/593655374469660673?label=Discord&logo=discord&style=flat
[discord-url]:  https://discord.gg/pb3U4zE8ca
[twitter-image]: https://img.shields.io/twitter/follow/web3_js?label=web3_js&style=social
[twitter-url]:  https://twitter.com/web3_js
[lerna-image]: https://img.shields.io/badge/maintained%20with-lerna-cc00ff.svg
[lerna-url]: https://lerna.js.org/
[netlify-image]: https://api.netlify.com/api/v1/badges/1fc64933-d170-4939-8bdb-508ecd205519/deploy-status
[netlify-url]: https://app.netlify.com/sites/web3-staging/deploys
[stackexchange-image]: https://img.shields.io/badge/web3js-stackexchange-brightgreen
[stackexchange-url]: https://ethereum.stackexchange.com/questions/tagged/web3js
[gitpoap-image]: https://public-api.gitpoap.io/v1/repo/ChainSafe/web3.js/badge
[gitpoap-url]: https://www.gitpoap.io/gh/ChainSafe/web3.js
[4x-release]: https://www.npmjs.com/package/web3
[4xdoc]: https://docs.web3js.org/
[cdnhits-image]: https://data.jsdelivr.com/v1/package/npm/web3/badge
[cdnhits-url]: https://www.jsdelivr.com/package/npm/web3

## Semantic versioning

This project follows [semver](https://semver.org/) as closely as possible **from version 1.3.0 onwards**. Earlier minor version bumps [might](https://github.com/ethereum/web3.js/issues/3758) have included breaking behavior changes.
>>>>>>>+origin/1.x
