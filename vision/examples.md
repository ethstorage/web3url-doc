# Examples

### Example 1: Fully decentralized exchange <a href="#b6a2" id="b6a2"></a>

Suppose a user wants to use a fully decentralized exchange in Web3, the user can do the following steps:

1. The user types a web3 address: **web3://uniswap.eth** in a web browser
2. The light client extension finds the contract address of uniswap.eth in ENS (suppose the result is 0xaabbccddee…)
3. The light client extension calls the contract 0xaabbccddee… (without parameters)
4. The EVM in the blockchain network responds with the content of the webpage in “\<HTML> … \</HTML>” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser

### Example 2: Fully decentralized social network <a href="#be8c" id="be8c"></a>

Suppose a user receives a link to a tweet, the user can read the tweet as follows.

1. The user types a web3 address: **web3://twitter.eth/view/1125236289743**
2. The light client extension finds the contract address of **twitter.eth** in ENS (suppose the result is 0x1122334455…)
3. The light client extension calls the contract 0x1122334455… with MethodID=”view” and Argument=uint256(1125236289743)
4. The EVM in the blockchain network responds with the content of the webpage in “\<HTML> … \</HTML>” with cryptographic proofs
5. The light client verifies the result and then displays it on the web browser.
