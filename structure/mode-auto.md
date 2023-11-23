# Resolve mode: auto

The smart contract is generic and has not implemented a interface defined by the ``web3://`` protocol. In this case, we will craft the URL path in a specific way to indicate the name, arguments and return signature of the method we want to call on the contract.

The path follows this structure:

```
/<methodName>/<methodArg1>/<methodArg2>/...[?returns=(<type1>,<type2>,...)]
```

## Method name

``<methodName>`` is the name of the function method to be called. Example : 

### Examples

```
web3://0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2/resourceName
```

This will call the ``resourceName`` method of the [``0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2``](https://etherscan.io/address/0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2) smart contract, and return ``???``.

> ⏩ Try now with a [web3:// gateway](https://0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2.w3eth.io/resourceName), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


## Arguments


``methodArg`` is an argument of the method with a syntax of ``[<type>!]<value>``. The protocol currently supports these basic types: bool, int, uint, int&lt;X&gt;, uint&lt;X&gt; (with X ranging from 8 to 256 in steps of 8), address, bytes&lt;X&gt; (with X ranging from 1 to 32), bytes, and string. If ``<type>!`` is not specified, then the type will be automatically detected using the following rule in a sequential way:

  1. **type**="uint256", if **value** is digits; or
  2. **type**="bytes32", if **value** is in the form of 0x+32-byte-data hex; or
  3. **type**="address", if **value** is in the form of 0x+20-byte-data hex; or
  4. **type**="bytes", if **value** is in the form of 0x followed by any number of bytes besides 20 or 32; or
  5. else **type**="address" and parse the argument as a domain name. If unable to resolve the domain name, an unsupported name service provider error will be returned. 

### Examples

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenHTML/9352
```

This URL will return the result of the ``tokenHTML(uint)`` method of the [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) smart contract in the Ethereum mainnet blockchain, using ``9352`` as an argument.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenHTML/9352), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


```
web3://0x36f379400de6c6bcdf4408b282f8b685c56adc60/getIdToSVG/uint8!42
```

In this example on the [``0x36f379400de6c6bcdf4408b282f8b685c56adc60``](https://etherscan.io/address/0x36f379400de6c6bcdf4408b282f8b685c56adc60) smart contract, the argument ``42`` is explicitely indicated to be an uint8.

> ⏩ Try now with a [web3:// gateway](https://0x36f379400de6c6bcdf4408b282f8b685c56adc60.w3eth.io/getIdToSVG/uint8!42), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)



## Method return signature

By default, when no method return signature is specified with ``?returns=``, the output of the smart contract is expected to be of type ``bytes`` (``string`` works too).

If the method return a non-``bytes`` type, or multiple values, we will use the ``?return=`` query to specify it. The returned data by the ``web3://`` protocol will then be encoded in JSON format.

The ``?return=`` query follows the syntax of the arguments part of the ethereum ABI function signature (``uint`` and ``int`` aliases are authorized). Sized/unsized arrays and structs are authorized.

Examples: ``(int)``, ``(string,int)``, ``(string[2],int[])``, ``(int,(string,int))``, ``(int,(string,int)[])``

The encoding of the returned data in the JSON will follow the Ethereum JSON-RPC format:
- Unformatted data (bytes, address) will be encoded as hex, prefixed with "0x", two hex digits per byte
- Quantities (integers) will be encoded as hex, prefix with "0x", the most compact representation (slight exception: zero should be represented as "0x0")
- Boolean and strings will be native JSON boolean and strings

### Examples

```
web3://0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2/levelAndTile/2/50?returns=(uint256,uint256)
```

In this example on the [``0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2``](https://etherscan.io/address/0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2) smart contract, we call the ``levelAndTile`` which returns 2 ``uint256``. It will return ``["0x1","0x24"]``.

> ⏩ Try now with a [web3:// gateway](https://0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2.w3eth.io/levelAndTile/2/50?returns=(uint256,uint256)), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenSupplementalData/9352?returns=((uint,uint,uint,uint,int,int,int,int,string,string[10],string[9]))
```

In this example on the [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) smart contract, we expect as a return a struct made of several fields. The output will be : 

```
[["0x0","0x7","0xc","0x5","0x3","0x4d9100","0x36f160","0xa5b354","First Earth",["#cb8175","#e2a97e","#f0cf8e","#f6edcd","#f6edcd","#a8c8a6","#a8c8a6","#6d8d8a","#655057","#32282b"],["█","▓","░","░","▒","▒","▒","▒","▓"]]]
```

### Advanced usage

If the ``?returns=`` attribute value is equal to "()", the raw bytes of the returned message data will be returned, encoded as a "0x"-prefixed hex string in an array in JSON format: ``["0xXXXXX"]``. You will then need to parse the raw EVM data. This is an advanced use and should be rarely used.


## MIME type

Similar to traditional ``https:// ``, the ``web3://`` protocol returns an HTTP status code and HTTP headers. The ``auto`` mode by default returns no HTTP headers, except in the following cases :

### File extension in last argument

If no ``?returns=`` attribute is set, and the last argument is of type string, and is in the format ``<text>.<ext>``, then if ``<ext>`` is a filename extension, the ``web3://`` protocol will returns a ``Content-Type`` header matching this file extension. Warning: the whole ``<text>.<ext>`` will be sent to the smart contract.

Theorical example: 


```
web3://xxxx/getFile/string!file.svg
```

In this example, the ``getFile`` method will be called with ``file.svg`` as an argument, and the ``web3://`` protocol will return the HTTP header ``Content-type: image/svg+xml``.

### ``?mime.content=`` and ``?mime.type=``

🟠 These two options are in PR pending status (ERC-7087) and could be modified, although unlikely.

If no ``?returns=`` attribute is set, you can either use :

- The ``?mime.content=`` attribute : This expect a MIME type such as ``image/svg+xml``. Note that, as in HTTP URLs, the attribute value are percent encoded, and in this example, it would be ``?mime.content=image/svg%2Bxml``.
- The ``?mime.type=`` attribute : This expect a file extension such as ``svg``

Example : 

```
web3://0x4e1f41613c9084fdb9e34e11fae9412427480e56/tokenSVG/9352?mime.type=svg
```

This URL will return the result of the ``tokenSVG(uint)`` method of the [``0x4e1f41613c9084fdb9e34e11fae9412427480e56``](https://etherscan.io/address/0x4e1f41613c9084fdb9e34e11fae9412427480e56) smart contract, and an HTTP header of ``Content-type: image/svg+xml``.

> ⏩ Try now with a [web3:// gateway](https://0x4e1f41613c9084fdb9e34e11fae9412427480e56.w3eth.io/tokenSVG/9352?mime.type=svg), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)