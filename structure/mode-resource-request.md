# Resolve mode: resource request

🟠 This resolve mode is in draft status (ERC-6944) and could be modified, although unlikely.

A ``resource request`` resolve mode smart contract is designed for the ``web3://`` protocol. In this case, any path is valid, and all the requests go through the ``request()`` method, which returns an HTTP status code, HTTP headers and the body.

Compared to the ``manual`` mode:

- It allows full control over the returned HTTP headers
- It allows control over the returned HTTP status code
- Basic parsing of the path (path parts, query key/values) is offloaded to the browser side.

### Example

```
web3://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656/
```

will call the ``request()`` method of the [``0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656``](https://etherscan.io/address/0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656) smart contract and return an HTML page, with links to other pages on the same domain, such as ``web3://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656/index/2``.

> ⏩ Try now with a [web3:// gateway](https://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656.w3eth.io/), or with the others ``web3://`` clients


## Declare a smart contract as resource request mode

To declare a smart contract as being of ``resource request`` mode, the smart contract has to implement this interface : 

```
    function resolveMode() external pure returns (bytes32) {
        return "5219";
    }
```


## Process page requests

In ``resource request`` mode, the path of the URL is processed on the browser side, and fed to the following interface (defined in ERC-5219) : 

```
struct KeyValue {
    string key;
    string value;
}

interface IDecentralizedApp {
    /// @notice                     Send an HTTP GET-like request to this contract
    /// @param  resource            The resource to request (e.g. "/asdf/1234" turns in to `["asdf", "1234"]`)
    /// @param  params              The query parameters. (e.g. "?asdf=1234&foo=bar" turns in to `[{ key: "asdf", value: "1234" }, { key: "foo", value: "bar" }]`)
    /// @return statusCode          The HTTP status code (e.g. 200)
    /// @return body                The body of the response
    /// @return headers             A list of header names (e.g. [{ key: "Content-Type", value: "application/json" }])
    function request(string[] memory resource, KeyValue[] memory params) external view returns (uint statusCode, string memory body, KeyValue[] memory headers);
}
```

Here is an example of smart contract using this interface : 

```
contract TerraformNavigator is IDecentralizedApp {
    // Implementation for the ERC-5219 mode
    function request(string[] memory resource, KeyValue[] memory params) external view returns (uint statusCode, string memory body, KeyValue[] memory headers) {

        // Frontpage
        if(resource.length == 0) {
            body = indexHTML(1);
            statusCode = 200;
            headers = new KeyValue[](1);
            headers[0].key = "Content-type";
            headers[0].value = "text/html";
        }
        // /index/[uint]
        else if(resource.length >= 1 && resource.length <= 2 && ToString.compare(resource[0], "index")) {
            uint page = 1;
            if(resource.length == 2) {
                page = ToString.stringToUint(resource[1]);
            }
            if(page == 0) {
                statusCode = 404;
            }
            else {
                body = indexHTML(page);
                statusCode = 200;
                headers = new KeyValue[](1);
                headers[0].key = "Content-type";
                headers[0].value = "text/html";
            }
        }
    }
}
```

## Chunk support

🟠 This feature is in PR pending status ([ERC-7617](https://github.com/ethereum/ERCs/pull/245/files)) and could be modified.

The resource you want to return may be so large you will run into the max gas limitation of the RPC provider used. To handle this problem, you can use the `chunking` feature: in your `request()` return, return a `web3-next-chunk` HTTP header with a `web3://` URL pointing to the next chunk of data. It will then loop until there is no more `web3-next-chunk` HTTP header returned. Please note that : 

- The contract pointed by the `web3://` URL must be also in ``resource request`` resolve mode.
- The `web3://` URL can be relative (e.g. `/xx/yy?a=b`) or absolute (e.g. `web3://zzzz/xx/yy?a=b`).

Hint: You can use solidity's `gasleft()` function to make dynamic chunks.

### Example

```
web3://0x8e990356262a2f8164981298e167c3ad2409faa1:11155111/getFile/abcd
```

- This will call the ``request()`` method of the [``0x8e990356262a2f8164981298e167c3ad2409faa1``](https://sepolia.etherscan.io/address/0x8e990356262a2f8164981298e167c3ad2409faa1) smart contract, and it will return a HTTP status code of `200`, the body `start`, and an `web3-next-chunk` HTTP header of value `/getFile/abcd?chunk=1`.
- The `200` HTTP status code, the HTTP headers (with `web3-next-chunk` removed) and the initial body chunk `start` is sent right away to the web client.
- The protocol then process the `web3://0x8e990356262a2f8164981298e167c3ad2409faa1:11155111/getFile/abcd?chunk=1` as it would a normal `web3://` URL, but it will only use the body and the `web3-next-chunk` HTTP header, if any, and ignore the rest. Here, the body is `middle` and the `web3-next-chunk` HTTP header is `/getFile/abcd?chunk=2`.
- The body chunk `middle` is streamed to the web client.
- The protocol then process the `web3://0x8e990356262a2f8164981298e167c3ad2409faa1:11155111/getFile/abcd?chunk=2`, and here the body is `end`, and there is no `web3-next-chunk` HTTP header.
- The body chunk `end` is streamed to the web client, and it indicate to the web client that there is no more data.

> ⏩ Try now with a [web3:// gateway](http://0x8e990356262a2f8164981298e167c3ad2409faa1.11155111.w3link.io/getFile/abcd), or with the others ``web3://`` clients


## Compression / Content-encoding support

🟠 This feature is in PR pending status ([ERC-7618](https://github.com/ethereum/ERCs/pull/246/files)) and could be modified.

To optimize blockchain storage cost, you may want to store compressed assets, and return a `Content-encoding` HTTP header of value `gzip` or `br` (brotli). In this case, the protocol will decompress the data, and return it to the client, and the `Content-encoding` HTTP header won't be returned to the client.

### Example

```
web3://0xb464cac335daec57d4f50657ec846c3109c774b2:3334/gzip.js
```

The smart contract ``0xb464cac335daec57d4f50657ec846c3109c774b2`` returns the string `console.log("hello world");` compressed in gzip, as well as a `Content-encoding: gzip` HTTP header. The `web3://` protocol, seeing the `Content-encoding` header, decompress and return the data, and remove the `Content-encoding` header from the ones forwarded to the web client.

> ⏩ Try now with a [web3:// gateway](http://0xb464cac335daec57d4f50657ec846c3109c774b2.3334.w3link.io/gzip.js), or with the others ``web3://`` clients

