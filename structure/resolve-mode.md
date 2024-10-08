# Resolve mode

To know how to interact with a smart contract, the ``web3://`` protocol needs to determine the **resolve mode** of a smart contract.

There are 3 resolve modes:

- **auto**: The smart contract is generic and has not implemented an interface defined by the ``web3://`` protocol. In this case, we will craft the URL path in a specific way to indicate the name, arguments and return signature of the method we want to call on the contract. [Read more about it](./mode-auto.md)
- **manual**: The smart contract has implemented a specific interface, and is now able to handle any path: ``/abcd``, ``/aa/bb/cc?xx=yy``, ... are valid paths. For each path, the smart contract is free to return a different result, like what a traditional web app would do. [Read more about it](./mode-manual.md)
- **resource request**: The smart contract has implemented a specific interface, and is now able to handle any path. The main differences with **manual** mode is that it allows more control over returned HTTP headers and status code, and most of the path processing (path splitting, query argument extracting) is done browser-side. 🟠 This resolve mode is in draft status (ERC-6944), but is not expected to evolve much. [Read more about it](./mode-resource-request.md)


## Resolve mode determination by clients

To determine the resolve mode of a smart contract, the ``web3://`` client will call first the smart contract with this method signature : 

```
function resolveMode() external returns (bytes32);
```

- The **manual** mode will be used if the `resolveMode` return value is `0x6d616e75616c0000000000000000000000000000000000000000000000000000`, i.e., "manual" in bytes32
- The **resource request** mode will be used if the `resolveMode` return value is `0x3532313900000000000000000000000000000000000000000000000000000000`, i.e., "5219" in bytes32
- The **auto mode** will be used if :
    - the `resolveMode` return value is `0x6175746f00000000000000000000000000000000000000000000000000000000`, i.e, "auto" in bytes32, or
    - the `resolveMode` return value is `0x0000000000000000000000000000000000000000000000000000000000000000`, or
    - the call to `resolveMode` throws an error (method not implemented or error thrown from the method)
- Otherwise, the protocol will fail the request with the error "unsupported resolve mode".


## Find out a contract resolve mode

You can use the [web3curl app](https://github.com/web3-protocol/web3curl-js) in verbose mode to determine the resolve mode of a smart contract. For example : 

```
web3curl -v 'web3://web3url.eth'
...
* Resolve mode determination... 
> 0xdd473fae
< 0x6d616e75616c0000000000000000000000000000000000000000000000000000
* Resolve mode: manual
...
```

In this example, we see that web3curl sent ``0xdd473fae`` to the smart contract, which is the call to ``resolveMode()``, and the smart contract returned ``0x6d616e75616c0000000000000000000000000000000000000000000000000000``, which, according to the rules above, let us know that the smart contract is in **manual** mode.
