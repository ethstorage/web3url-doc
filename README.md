# Web3 protocol

``web3://`` is a new protocol to access content returned by smartcontracts in EVM-compatible blockchains. 

It is meant to be similar to the ``https://`` protocol : one can browse websites and download content using their web browser, CURL-like apps, ... except that the content come from blockchains.

Web3 URLs are very similar in structure to traditional ``https://`` URLs, and have been defined in several [Ethereum ERC standards](structure/base.md).

Learn more about [the ``web3://`` vision](./vision/vision.md)


## Examples

#### Access an on-chain website

```
web3://w3url.eth
```

This on-chain website is located in the ``0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6`` smartcontract in the W3Q testnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://w3url.w3eth.io), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


#### Get a NFT

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenHTML/9352
```

This URL will fetch the HTML of the NFT number 9352 of the Terraforms NFT collection located at [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) on the Ethereum mainnet blockchain.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenHTML/9352), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


#### Fetch an USDC balance

```
web3://usdc.eth/balanceOf/nemorino.eth?returns=(uint256)
```

This URL will fetch the balance of USDC of the account ``nemorino.eth``.

> ⏩ Try now with a [web3:// gateway](https://usdc.w3eth.io/balanceOf/nemorino.eth?returns=(uint256)), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)



## 2 categories of smartcontracts

Before fetching data, the ``web3://`` protocol determine the **resolve mode** of the smartcontract, to know how to deal with it. Learn more about [resolve modes](structure/resolve-mode.md).

### Generic smartcontracts

The vast majority of existing smartcontracts were not specifically designed for the ``web3://`` protocol and thus the **resolve mode** will be ``auto``.

In this mode, the URL is structured to identify the smartcontract method to call, and the structure of the returned data. ``web3://usdc.eth/balanceOf/nemorino.eth?returns=(uint256)`` is an example of URL with a **resolve mode** of ``auto``.

Learn more about [auto mode URLs](structure/mode-auto.md)

### Smartcontracts designed for ``web3://``

When a smartcontract is designed for the ``web3://`` protocol, it will implement a specific interface, and there is much more freedom with URL paths.

Two differents **resolve modes** exists for this scenario: ``manual`` and ``resource request``. ``web3://w3url.eth`` is an example of URL with a **resolve mode** of ``manual``.

Learn more about [manual mode URLs](structure/mode-manual.md) and [5219 mode URLs](structure/mode-auto.md)