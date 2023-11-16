# Domain name resolution

The ``web3://`` protocol support at least the Ethereum Name Service (ENS) domain name service, but other domain name services could be supported depending of the clients (W3NS name service from the Galileo blockchain, Linagee .og name service in Ethereum mainnet, ...)

The base resolution supported is a simple domain name to address resolution.


### Advanced cross-chain name resolution

🟠 This cross-chain mechanism is in draft status (ERC-6821) but is unlikely to change much.

For the Ethereum Name Service, this functionality use a special TXT field, ``contentcontract``, filled with a ERC-3770 chain-specific address in the format ``<chainShortName>:<address>``. (Short names are defined in https://github.com/ethereum-lists/chains and the aggregated definitions can be accessed at https://chainid.network/chains.json ).

For example:

```
web3://w3url.eth
```

The domain name ``w3url.eth`` has a ``contentcontract`` TXT field with the value ``w3q-g:0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6``, which indicate that the smartcontract to request in at the address ``0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6`` on the blockchain identified by his short name ``w3q-g``.


### Find out how a domain name was resolved

You can use the [web3curl app](https://github.com/web3-protocol/web3curl-js) in verbose mode to determine the domain name resolution of a smartcontract. For example : 

```
node . -v 'web3://w3url.eth'
...
* Host domain name resolver: ens
*   Resolver address: 0xc0497E381f536Be9ce14B0dD3817cBcAe57d2F62
*   Resolver chain id: 1
*   Resolver chain RPC: https://ethereum.publicnode.com
*   Domain name being resolved: w3url.eth
*   Resolution type: contentContractTxt
*   contentcontract TXT record: w3q-g:0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6
*   Result address: 0xEbcA4860ebBe969E9Bc42643fcb437879dBDa9C6
*   Result chain id: 3334
...
```
