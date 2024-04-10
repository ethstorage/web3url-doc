# FlatDirectory

`FlatDirectory` proposed in [ERC-5018](https://eips.ethereum.org/EIPS/eip-5018) is a standard interface for file system directories that allows any binary objects on EVM-based blockchain to be re-used by other dApps.

The following standard allows for implementing a standard API for file system directories within smart contracts. This standard provides basic functionality to read/write binary objects for any size and allows reading/writing chunks of the object if the object is too large to fit in a single transaction.

For more information, you can check out the [ERC-5018](https://eips.ethereum.org/EIPS/eip-5018).

### Proposed APIs

**write**

Writes binary `data` to the file `name` in the directory by an account with write permission.

```
function write(bytes memory name, bytes memory data) external payable
```

**read**

Returns the binary `data` from the file `name` in the directory and existence of the file.

```
function read(bytes memory name) external view returns (bytes memory, bool)
```

**size**

Returns the size of the `data` from the file `name` in the directory and the number of chunks of the data.

```
function size(bytes memory name) external view returns (uint256, uint256)
```

**remove**

Removes the file `name` in the directory and returns the number of chunks removed (0 means the file does not exist) by an account with write permission.

```
function remove(bytes memory name) external returns (uint256)
```

**countChunks**

Returns the number of chunks of the file `name`.

```
function countChunks(bytes memory name) external view returns (uint256);
```

**writeChunk**

Writes a chunk of data to the file by an account with write permission. The write will fail if `chunkId > numOfChunks`, i.e., the write must append the file or replace the existing chunk.

```
 function writeChunk(bytes memory name, uint256 chunkId, bytes memory data) external payable;
```

**readChunk**

Returns the chunk data of the file `name` and the existence of the chunk.

```
function readChunk(bytes memory name, uint256 chunkId) external view returns (bytes memory, bool);
```

**chunkSize**

Returns the size of a chunk of the file `name` and the existence of the chunk.

```
function chunkSize(bytes memory name, uint256 chunkId) external view returns (uint256, bool);
```

**removeChunk**

Removes a chunk of the file `name` and returns `false` if such chunk does not exist. The method should be called by an account with write permission.

```
function removeChunk(bytes memory name, uint256 chunkId) external returns (bool);
```

### Implementation

An example of the implementation can be found [here](https://github.com/ethstorage/evm-large-storage/blob/master/contracts/examples/FlatDirectory.sol).

## Resolve Modes in FlatDirectory

### Resolve Mode in Contracts

When implementing your own web handler as a smart contract, there are some engineering intricacies worth noting, especially the [**resolve modes**](../structure/resolve-mode.md).

When integrated with Web3URL Gateway, the resolve mode will determine how an API call will be translated into onchain calldata for information queries. Currently, two different resolve modes are supported, _auto_ and _manual_ - the former is the easiest to use while the latter gives more flexibility.

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
