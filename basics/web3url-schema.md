# Web3URL Schema

As mentioned in our [solution](../overview/our-solution.md), the long-term solution for accessing the blockchain will be via browser extensions serving as _light clients_ directly fetching on-chain data. On the other hand, the short-term approach we chose to implement is via a dedicated server which we will refer to as the _Web3URL gateway_, and it essentially acts like a translation layer between HTTP-style URL queries and on-chain EVM _call_ queries. For more, feel free to check out [our original proposal](https://github.com/ethstorage/W3IPs/blob/main/W3IPS/w3ip-1.md).

## Web3 URL to EVM Call Message Translation

### Simple Summary

A translation of an HTTP-style Web3 URL to an EVM call message

### Abstract

The standard translates an HTTP-style Web3 URL like

```
web3://uniswap.eth/...
```

to an EVM message such as

```
EVMMessage {
   To: 0xaabbccddee.... // Name resolution follows the proposed EIP-6821
   Calldata: 0x...
}
```

### Motivation

Currently, reading data from Web3 generally relies on a translation done by a Web2 proxy to Web3 blockchain. The translation is mostly done by the proxies such as dApp websites/node service provider/etherscan, which are out of the control of users. The standard here aims to provide a simple way for Web2 users to directly access the content of Web3.

### Specification

The specification here only specifies for the read EVM message. Write message will be defined in the future proposals.

A Web3 URL is in the following form

```
Web3URL = "web3://" [userinfo "@"] contractName [":" chainid] path ["?" query]
contractName = address | name "." nsProvider
path = ["/" method ["/" argument_0 ["/" argument_1 ... ]]]
argument = [type "!"] value
query = ["returns=(" returnTypes ")"]
returnTypes = returnType1 ["," returnType2 ...]
```

where

* "web3://" indicates the Web3 URL **schema**.
* **userinfo** indicates which user is calling the EVM, i.e., "From" field in EVM call message. If not specified, the protocol will use 0x0 as the sender address.
* **contractName** indicates the contract to be called, i.e., "To" field in the EVM call message. If the **contractName** is an **address**, i.e., 0x + 20-byte-data hex, then "To" will be the address. Otherwise, the name is from a name service. In the second case, **nsProvider** will be the short name of name service providers such as "ens", "w3q", etc. For ENS, the resolution happens via the [proposed EIP-6821](https://ethereum-magicians.org/t/eip-6821-support-ens-name-for-web3-url/13654). For others name providers, it will be defined in future proposals.
* **chainid** indicates which chain to call the message. If not specified, the protocol will use the same chain as the name service provider, e.g., 1 for eth, and 333 for w3q. If no name service provider is available, the default chainid is 1.
* **returnTypes** tells the format of the returned data. If not specified, the returned message data will be parsed as abi.encode()'d bytes and MIME will be set based on the suffix of the last argument. If **returnTypes** is "()", the returned data will be parsed in raw bytes in JSON. Otherwise, the returned message will be parsed in the specified "returnTypes" in JSON.

#### Resolver Mode

Once the "To" address and chainid are determined, the protocol will check the resolver mode of contract by calling "resolveMode" method. The protocol currently supports two resolver modes:

**Manual Mode**

The manual mode will not do any interpretation of **path** "?" **query**, with the exception of the determination of MIME type based on the suffix of the **path**, and put **path** "?" **query** as calldata of the message directly.

**Auto Mode**

The auto mode is the default mode of resolver (also applies when the "resolveMode" method is unavailable in the target contract). In the auto mode, if **path** is empty, then the protocol will call the target contract with 0x2f ("/") calldata. Otherwise, the calldata of the EVM message will use standard Ethereum contract ABI [Contract ABI Specification — Solidity 0.8.3 documentation](https://docs.soliditylang.org/en/v0.8.3/abi-spec.html), where

* **method** is a string of function method be called
* **argument\_i** is the ith argument of the method. If **type** is specified, the value will be translated to the corresponding type. The protocol currently supports the basic types such as uint256, bytes32, address, and bytes. If **type** is not specified, then the type will be automatically detected using the following rule in a sequential way:

1. **type** = "uint256", if **value** is numeric; or
2. **type** = "bytes32", if **value** is in the form of 0x+32-byte-data hex; or
3. **type** = "address", if **value** is in the form of 0x+20-byte-data hex; or
4. **type** = "name", if **value** is in the form of **name**.**nsProvider**. In this case, the actual value of the argument will be **address**, which is obtained from **nsProvider**, e.g., eth, w3q, etc.
5. else **type**="bytes"

Note that if **method** does not exist, i.e., **path** is empty or "/", then the contract will be called with 0x2f ("/") calldata.

### Examples

**Example 1**

```
web3://uniswap.eth
```

The protocol will find the address of **uniswap.eth** from ENS via the [proposed EIP-6821](https://ethereum-magicians.org/t/eip-6821-support-ens-name-for-web3-url/13654): in this case, no contentcontract TXT field, so it does a normal ENS resolution. Then it call the address with "From" = "0x00…0" and "Calldata" = "0x2f" with chainid = 1.

**Example 2**

```
web3://vitalikblog.eth
```

The protocol will find the address of **vitalikblog.eth** from ENS via the [proposed EIP-6821](https://ethereum-magicians.org/t/eip-6821-support-ens-name-for-web3-url/13654): in this case, there is a contentcontract TXT field of value "arb-nova:0xe4ba0e245436b737468c206ab5c8f4950597ab7f". Then it call the address with "From" = "0x00…0", "to" = "0xe4ba0e245436b737468c206ab5c8f4950597ab7f" "Calldata" = "0x2f" with chainid = 42170 (arbitrum nova).

**Example 3**

```
web3://opensea.eth:5
```

The protocol will find the address of **opensea.eth** from the goerli ENS via the [proposed EIP-6821](https://ethereum-magicians.org/t/eip-6821-support-ens-name-for-web3-url/13654): in this case, no contentcontract TXT field, so it does a normal ENS resolution. Then it call the address with "From" = "0x00…0" and "Calldata" = "0x2f" with chainid = 5.

**Example 4**

```
web3://twitter.w3q/view/15235234234234
```

The protocol will find the address of **twitter.w3q** from W3NS, and then call the address with "From" = "0x00…0" and "Calldata" = "0x" + keccak("view(uint256)")\[0:4] + abi.encode(uint256(15235234234234)) with chainid = 333.

**Exmaple 5**

```
web3://qizhou.w3q/files/index.html
```

The protocol will find the address of **qizhou.w3q** from W3NS, and then call the address with "From" = "0x00…0" and "Calldata" = "0x" + keccak("files(bytes)")\[0:4] + abi.encode(bytes("/index.html")) with chainid = 333.

**Example 6**

```
web3://usdc.eth/balanceOf/0x1122...ff?returns=(uint256)
```

The protocol will find the address of **usdc.eth** and then call the method "balanceOf(address)" of the address. The returned data will be parsed as uint256 as `[ "523576225" ]`.

**Example 7**

```
web3://usdc.eth/balanceOf/0x1122...ff?returns=()
```

The protocol will find the address of **usdc.eth** and then call the method "balanceOf(address)" of the address. The returned data will be parsed as raw bytes as `[ "0x000000000000000000000000000000000000000000000000000000001f3523a1" ]`.
