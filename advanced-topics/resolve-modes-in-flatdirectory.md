# Resolve Modes in FlatDirectory

### Resolve Mode in Contracts

When implementing your own web handler on Galileo as a smart contract, there are some engineering intricacies worth noting, especially the [**resolve modes**](../basics/web3url-schema.md#resolver-mode).

When integrated with Web3URL Gateway, the resolve mode will determine how an API call will be translated into onchain calldata for information queries. Currently two different resolve modes are supported, _auto_ and _manual_ - the former is the easiest to use while the latter gives more flexibility.

For example, the following contract specifies _auto_ resolve mode:

```
contract SimpleFlatDirectory {

    function resolveMode() external pure virtual returns (bytes32) {
        return "auto";
    }

    function files(bytes memory filename) public view returns (bytes memory) {
        bytes32 b32 = bytesToBytes32(filename);
        (bytes memory data, ) = _get(b32);
        return data;
    }
    
    ...
}
```

(Note the `resolveMode` function, it's strongly suggested that your web handler should always have this function explicitly.) When interacting with this contract using Web3URL Gateway, URLs such as `https://<contract address>.<chain id>.<gateway>/files/index.html` will be translated into `contract.files("index.html")`. The gateway essentially helps parse the URL to corresponding function calls.

On the other hand, if the contract specifies _manual resolve mode:_

```
contract SimpleFlatDirectory {

    function resolveMode() external pure override returns (bytes32) {
        return "manual";
    }

    fallback(bytes calldata cdata) external override returns (bytes memory) {
        bytes memory content;
        if (cdata.length == 0) {
          return bytes("");
        } else if (cdata[0] != 0x2f) {
            // Should not happen since manual mode will have prefix "/" like "/....."
            return bytes("incorrect path");
        }

        if (cdata.length == 1) {
            content = files(defaultFile);
        } else {
            content = files(cdata[1:]);
        }

        bytes memory returnData = abi.encode(content);
        return returnData;
    }
    
    ...
}
```

Then the same URL `https://<contract address>.<chain id>.<gateway>/files/index.html` will directly put `/files/index.html` as calldata into the function call, while your web handler is expected to do the parsing. Therefore in this example, the correct way to invoke the `files` method should be `https://<contract address>.<chain id>.<gateway>/index.html` without the "files" path.

In short, _auto_ resolve mode should suffice in most use cases, which establishes 1-to-1 relationship between the gateway URLs and the contract method, but if your web handler needs to take care of more complicated queries (such as nested paths) then _manual_ resolve may be more suitable.
