# Web3 protocol

``web3://`` is a new protocol to access content returned by smart contracts in EVM-compatible blockchains.

It is meant to be similar to the ``https://`` protocol : one can browse websites and download content using their web browser, CURL-like apps, ... except that the content come from blockchains.

Web3 URLs are very similar in structure to traditional ``https://`` URLs, and have been defined in several [Ethereum ERC standards](structure/base.md).

Learn more about [the ``web3://`` vision](./vision/vision.md)


## Examples

#### Access an on-chain website

```
web3://web3url.eth
```

This on-chain website is located in the ``0x5ad14e8439b9619e165db27545faf6df13e2b947`` smart contract in the QuarkChain L2 testnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://web3url.w3eth.io), or with the others ``web3://`` clients


#### Get a NFT

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenHTML/9352
```

This URL will fetch the HTML of the NFT number 9352 of the Terraforms NFT collection located at [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) on the Ethereum mainnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenHTML/9352), or with the others ``web3://`` clients


#### Fetch an USDC balance

```
web3://0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48/balanceOf/nemorino.eth?returns=(uint256)
```

This URL will fetch the balance of USDC of the account ``nemorino.eth``.

> ⏩ Try now with a [web3:// gateway](https://0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48.w3eth.io/balanceOf/nemorino.eth?returns=(uint256)), or with the others ``web3://`` clients



## 2 categories of smart contracts

Before fetching data, the ``web3://`` protocol determine the **resolve mode** of the smart contract, to know how to deal with it. Learn more about [resolve modes](structure/resolve-mode.md).

### Generic smart contracts

The vast majority of existing smart contracts were not specifically designed for the ``web3://`` protocol and thus the **resolve mode** will be ``auto``.

In this mode, the URL is structured to identify the smart contract method to call, and the structure of the returned data. ``web3://0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48/balanceOf/nemorino.eth?returns=(uint256)`` is an example of URL with a **resolve mode** of ``auto``.

Learn more about [auto mode URLs](structure/mode-auto.md)

### Smart contracts designed for ``web3://``

When a smart contract is designed for the ``web3://`` protocol, it will implement a specific interface, and there is much more freedom with URL paths.

Two differents **resolve modes** exists for this scenario: ``manual`` and ``resource request``. ``web3://web3url.eth`` is an example of URL with a **resolve mode** of ``manual``.

Learn more about [manual mode URLs](structure/mode-manual.md) and [5219 mode URLs](structure/mode-resource-request.md)
