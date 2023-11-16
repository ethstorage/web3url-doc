# Web3 protocol

``web3://`` is a new protocol to access content returned by smartcontracts in EVM-compatible blockchains. 

It is meant to be the equivalent of the ``https://`` protocol : one can browse websites and download content from their web browser, CURL-like apps, ...

Web3 URLs are very similar in structure to traditional ``https://`` URLs, and have been defined in several Ethereum ERC standards.

Learn more about [the ``web3://`` vision](./vision/vision.md)


## Examples

#### Get a NFT

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenHTML/9352
```

This URL will return the result of the ``tokenHTML(uint)`` method of the ``0x4e1f41613c9084fdb9e34e11fae9412427480e56`` smartcontract in the Ethereum mainnet blockchain, using ``9352`` as an argument.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenHTML/9352), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)

#### Fetch an USDC balance

```
web3://usdc.eth/balanceOf/nemorino.eth?returns=(uint256)
```

This URL will return the result of the ``balanceOf(adress)`` method of the ``0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48`` smartcontract (resolved from ``usdc.eth``), using ``0x8AeCc5526F92A46718f8E68516D22038D8670E0D`` as argument (resolved from ``nemorino.eth``)

> ⏩ Try now with a [web3:// gateway](https://usdc.w3eth.io/balanceOf/nemorino.eth?returns=(uint256)), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)

#### Access an on-chain website

```
web3://w3url.eth
```

More advanced case: The domain name ``w3url.eth`` will resolve into ``0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6`` in the W3Q testnet blockchain, and it will return the HTML of the webpage (more details later).

> ⏩ Try now with a [web3:// gateway](https://w3url.w3eth.io), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


## 2 categories of smartcontracts

Before fetching data, the ``web3://`` protocol determine the **resolve mode** of the smartcontract, to know how to deal with it.

### Generic smartcontracts

The vast majority of existing smartcontracts were not specifically made for the ``web3://`` protocol and thus the **resolve mode** will be ``auto``.

In this mode, the URL is structured to identify the smartcontract method to call, and the structure of the returned data. ``web3://usdc.eth/balanceOf/nemorino.eth?returns=(uint256)`` is an example of URL with a **resolve mode** of ``auto``.

[Learn more about auto mode URLs](xx)

### Smartcontracts designed for ``web3://``

In this scenario, the smartcontracts implement a specific interface, and there is much more freedom with URL paths. 

Two differents **resolve modes** exists for this scenario: ``manual`` and ``resource request``. ``web3://w3url.eth`` is an example of URL with a **resolve mode** of ``manual``.

[Learn more about manual and 5219 modes](xx)