# Resolve mode: manual

A ``manual`` resolve mode smart contract is designed for the ``web3://`` protocol. In this case, any path is valid, and the smart contract will usually returned some content for at least the root path.

For example:

```
web3://terraformnavigator.eth/
```

will call the [``0xAD41bf1c7f22F0ec988DaC4C0aE79119Cab9BB7E``](https://etherscan.io/address/0xAD41bf1c7f22F0ec988DaC4C0aE79119Cab9BB7E) smart contract and return an HTML page, with links to other pages on the same domain, such as ``web3://terraformnavigator.eth/index/2``.

> ⏩ Try now with a [web3:// gateway](https://terraformnavigator.w3eth.io/), or with the others ``web3://`` clients


## Declare a smart contract as manual mode

To declare a smart contract as being of ``manual`` mode, the smart contract has to implement this interface : 

```
    function resolveMode() external pure returns (bytes32) {
        return "manual";
    }
```

## Process page requests

In ``manual`` mode, the whole path of the ``web3://`` URL, unmodified, is sent as calldata to the smart contract, which can process it in his ``fallback(bytes)`` method. The returned data MUST be ``abi.encode``-d ``bytes``. The path sent as calldata is always at minimum ``/``. Here is an example of an implementation of a router for the smart contract: 

```
    fallback(bytes calldata cdata) external returns (bytes memory) {
        if(cdata.length == 0 || cdata[0] != 0x2f) {
            return bytes("");
        }

        // Frontpage call
        if (cdata.length == 1) {
          return bytes(abi.encode(indexHTML(1)));
        }
        // /index/[uint]
        else if(cdata.length >= 6 && ToString.compare(string(cdata[1:6]), "index")) {
            uint page = 1;
            if(cdata.length >= 8) {
                page = ToString.stringToUint(string(cdata[7:]));
            }
            if(page == 0) {
                return abi.encode("Not found");
            }
            return abi.encode(indexHTML(page));
        }

        // Default
        return abi.encode("Not found");
    }
```


## MIME type

Similar to traditional ``https:// ``, the ``web3://`` protocol returns an HTTP status code and HTTP headers. The ``auto`` mode by default returns the ``Content-type: text/html`` header.

If the path end with ``.<extension>`` (query excluded), then a MIME type will be tried to be determined from it, and used instead of ``text/html``. Example : 

```
web3://terraformnavigator.eth/view/9352.svg
```

In this URL, it will call the [``0xAD41bf1c7f22F0ec988DaC4C0aE79119Cab9BB7E``](https://etherscan.io/address/0xAD41bf1c7f22F0ec988DaC4C0aE79119Cab9BB7E) smart contract and send ``/view/9352.svg`` as calldata. Because the path ends with ``.svg`` and ``svg`` correspond to the ``image/svg+xml`` MIME type, the returned header will be ``Content-type: image/svg+xml``.

> ⏩ Try now with a [web3:// gateway](https://terraformnavigator.w3eth.io/view/9352.svg), or with the others ``web3://`` clients