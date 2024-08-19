# Web3curl

[web3curl](https://github.com/web3-protocol/web3curl-js) is a curl-like app to download data from an ``web3://`` URL. It can be very useful to help debugging URLs.

## Basic usage

```bash
web3curl "web3://0xA5aFC9fE76a28fB12C60954Ed6e2e5f8ceF64Ff2/resourceName"
```

Calling this above will output ``???``.


## Debugging with web3curl

Use the verbose mode option ``-v`` to have plenty of informations on what is going on. For example : 

```bash
web3curl -v 'web3://web3url.eth'
```

will output

```
* Fetching URL web3://web3url.eth
* Parsing URL ...
* Host domain name resolver: ens
*   Resolver address: 0xc0497E381f536Be9ce14B0dD3817cBcAe57d2F62
*   Resolver chain id: 1
*   Resolver chain RPC: https://ethereum.publicnode.com
*   Domain name being resolved: web3url.eth
*   Resolution type: contentContractTxt
*   contentcontract TXT record: esl2-t:0x5ad14e8439b9619e165db27545faf6df13e2b947
*   Result address: 0x5ad14e8439b9619e165db27545faf6df13e2b947
*   Result chain id: 3336
* Contract address: 0x5ad14e8439b9619e165db27545faf6df13e2b947
* Contract chain id: 3336
* Resolve mode determination... 
> 0xdd473fae
< 0x6d616e75616c0000000000000000000000000000000000000000000000000000
* Resolve mode: manual
* Contract call mode: calldata
* Calldata: 0x2f
* Contract return processing: decodeABIEncodedBytes
* Contract return processing: decodeABIEncodedBytes: MIME type: text/html
*
* Calling contract ...
* RPC: https://rpc.testnet.l2.ethstorage.io:9540
* Contract address: 0x5ad14e8439b9619e165db27545faf6df13e2b947
> 0x2f
< 0x00000000000....0000000000000126
*
* Decoding contract return ...
* HTTP Status code: 200
* HTTP Headers: 
*   Content-Type: text/html
<!DOCTYPE html><html lang="">....</html>
```
