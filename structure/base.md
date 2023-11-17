# Basic structure

## Standards

🟠 The ``web3://`` protocol is made of various Ethereum ERCs, some of which are still in draft status. We will flag on this documentation what is definitive and what could change.

Here is the list of ERCs : 

- [ERC-4804](https://eips.ethereum.org/EIPS/eip-4804): The base ERC from which everything is based on. This ERC is final and cannot be edited anymore.
- [ERC-6860](https://eips.ethereum.org/EIPS/eip-6860): (Draft) This ERC updates the base [ERC-4804](https://eips.ethereum.org/EIPS/eip-4804) with clarifications, minor fixes and changes. Still in draft status.
- [ERC-6821](https://eips.ethereum.org/EIPS/eip-6821): (Draft) ENS resolution : support for the ``contentcontract`` TXT field to point to a contract in another chain. Still in draft status.
- [ERC-6944](https://eips.ethereum.org/EIPS/eip-6944): (Draft) New resolve mode offloading some parsing processing on the browser side, based on [ERC-5219](https://eips.ethereum.org/EIPS/eip-5219). Still in draft status.
- [ERC-7087](https://github.com/ethereum/ERCs/pull/98): (PR pending) Auto mode : Add new features for auto mode.


## Base structure

```
web3://<contract>[:<chainId>]/<path>
```

The URLs are following a structure close to traditional HTTP URLs:

- ``<contract>`` can either be an contract address such as ``0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2`` or a domain name such as ``w3url.eth``. Learn more about [domain name resolution](./domain-name.md).
- ``chainId`` is optional and indicate the chain id of the blockchain where to query the smartcontract. ``web3://0x5a985f13345e820aa9618826b85f74c3986e1463:5/tokenHTML/2`` will for example query on the goerli blockchain (chain id = 5).
- ``path`` follows a similar structure than traditional HTTP URLs, in the form of ``/path/path2?query1=xx&query2=xx``. To know how to build a path, we first need to know the [**resolve mode** of the called smartcontract](./resolve-mode.md)