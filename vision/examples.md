# Examples

### Example 1: Fully decentralized name service <a href="#b6a2" id="b6a2"></a>

Suppose a user wants to use a fully decentralized name service in Web3, the user can do the following steps:

1. The user types a web3 address: **web3://app-ens-domain.eth** in a web browser
2. The light client extension finds the contract address of app-ens-domain.eth in ENS (suppose the result is 0xaabbccddee…)
3. The light client extension calls the contract 0xaabbccddee… (without parameters)
4. The EVM in the blockchain network responds with the content of the webpage in “\<HTML> … \</HTML>” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser

### Example 2: Decentralized access to NFTs <a href="#be8c" id="be8c"></a>

Suppose a user wants to access a specific NFT from the collection; they can follow these steps:

1. The user types a web3 address: **web3://cyberbrokers-meta.eth/renderBroker/5**
2. The light client extension finds the contract address of **cyberbrokers-meta.eth** in ENS (suppose the result is 0x1122334455…)
3. The light client extension calls the contract 0x1122334455… with MethodID=”renderBroker” and Argument=uint256(5)
4. The EVM in the blockchain network responds with the content of the webpage in “\<svg> … \</svg>” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser.
