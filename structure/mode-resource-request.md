# Resolve mode: manual

🟠 This resolve mode is in draft status (ERC-6944) and could be modified, although unlikely.

A ``resource request`` resolve mode smartcontract is designed for the ``web3://`` protocol. In this case, any path is valid, and the smartcontract will usually returned some content for at least the root path.

Compared to the ``manual`` mode:

- It allows full control over the returned HTTP headers
- It allows control over the returned HTTP status code
- Basic parsing of the path (path parts, query key/values) is offloaded to the browser side.

For example:

```
web3://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656/
```

will call the [``0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656``](https://etherscan.io/address/0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656) smartcontract and return an HTML page, with links to other pages on the same domain, such as ``web3://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656/index/2``.

> ⏩ Try now with a [web3:// gateway](https://0x2b51a751d3c7d3554e28dc72c3b032e5f56aa656.w3eth.io/), with the [EVM browser](https://github.com/nand2/ethereum-browser) or the [web3curl app](https://github.com/web3-protocol/web3curl-js)


## Declare a smartcontract as resource request mode

To declare a smartcontract as being of ``resource request`` mode, the smartcontract has to implement this interface : 

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

Here is an example of smartcontract using this interface : 

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