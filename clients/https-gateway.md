# HTTPS gateways

## web3:// Gateway

[web3url-gateway](https://github.com/ethstorage/web3url-gateway) is an opensource gateway for ``web3://`` URLs. It works by translating HTTPS URLs into ``web3://`` URLs, then process them using the [web3protocol-go engine](https://github.com/web3-protocol/web3protocol-go).

It handles two modes : all blockchains, or single-blockchain.

### All blockchains support

In this case, assuming a gateway domain of ``example.com``, the URLs will be :

| web3 URL                                                    | HTTPS URL                                                                |
|-------------------------------------------------------------|--------------------------------------------------------------------------|
| ``web3://<domainName>.<domainNameSuffix>/<path>``           | ``https://<domainName>.<domainNameSuffix>.1.example.com/<path>``         |
| ``web3://<contractAddress>/<path>``                         | ``https://<contractAddress>.1.example.com/<path>``                       |
| ``web3://<domainName>.<domainNameSuffix>:<chainId>/<path>`` | ``https://<domainName>.<domainNameSuffix>.<chaidId>.example.com/<path>`` |
| ``web3://<contractAddress>:<chainId>/<path>``               | ``https://<contractAddress>.<chaidId>.example.com/<path>``               |

### Single blockchain support

In this case, assuming a gateway domain of ``ethexample.com``, and the blockchain being ethereum mainnet, the URLs will be :

| web3 URL                                          | HTTPS URL                                           |
|---------------------------------------------------|-----------------------------------------------------|
| ``web3://<domainName>.<domainNameSuffix>/<path>`` | ``https://<domainName>.ethexample.com/<path>``      |
| ``web3://<contractAddress>/<path>``               | ``https://<contractAddress>.ethexample.com/<path>`` |


## w3link.io all-blockchains public gateway

Provided by the [EthStorage](https://eth-store.w3eth.io) team, the w3link.io gateway is an all-blockchain public gateway. It supports the following formats:

* ``https://<domainName>.<domainNameSuffix>.<chainIdOrShortName>.w3link.io``
* ``https://<contractAddress>.<chainIdOrShortName>.w3link.io``

Supported networks are listed here : 

| Chain Name                 | Chain Short Name and Chain Id | Samples                                                                                                                                                                                                          |
| -------------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ethereum Mainnet           | eth / 1                       | [https://cyberbrokers-meta.eth.1.w3link.io/renderBroker/5](https://cyberbrokers-meta.eth.1.w3link.io/renderBroker/5)                                                                                             |
| Goerli Testnet             | gor / 5                       | [https://ethstorage.eth.5.w3link.io/hello.txt](https://ethstorage.eth.5.w3link.io/hello.txt)                                                                                                                     |
| Sepolia Testnet            | sep / 11155111                | [https://0x9616fd0f0afc5d39c518289d1c1189a50bde94f5.sep.w3link.io/name?returns=(string)](https://0x9616fd0f0afc5d39c518289d1c1189a50bde94f5.sep.w3link.io/name?returns=\(string\))                               |
| Optimism                   | oeth / 10                     | [https://0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1.oeth.w3link.io/name?returns=(string)](https://0xda10009cbd5d07dd0cecc66161fc93d7c9000da1.oeth.w3link.io/name?returns=\(string\))                             |
| Optimism Testnet           | ogor / 420                    | [https://0xDA10009cBd5D07dd0CeCc66161FC93D7c9000da1.ogor.w3link.io/name?returns=(string)](https://0xda10009cbd5d07dd0cecc66161fc93d7c9000da1.ogor.w3link.io/name?returns=\(string\))                             |
| Arbitrum One               | arb1 / 42161                  | [https://0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8.arb1.w3link.io/name?returns=(string)](https://0xff970a61a04b1ca14834a43f5de4533ebddb5cc8.arb1.w3link.io/name?returns=\(string\))                             |
| Arbitrum Nova              | arb-nova / 42170              | [https://0x2ce21976443622ab8f0b7f6fa3af953ff9bcdcf6.arb-nova.w3link.io/name?returns=(string)](https://0x2ce21976443622ab8f0b7f6fa3af953ff9bcdcf6.arb-nova.w3link.io/name?returns=\(string\))                     |
| Arbitrum Testnet           | arb-goerli / 421613           | [https://0x0997fb92ee366c93d66fF43ba337ACA94F56EAe0.arb-goerli.w3link.io/totalSupply?returns=(uint256)](https://0x0997fb92ee366c93d66ff43ba337aca94f56eae0.arb-goerli.w3link.io/totalSupply?returns=\(uint256\)) |
| Metis Andromeda            | metis-andromeda / 1088        | [https://0xdeaddeaddeaddeaddeaddeaddeaddeaddead0000.metis-andromeda.w3link.io/name?returns=(string)](https://0xdeaddeaddeaddeaddeaddeaddeaddeaddead0000.metis-andromeda.w3link.io/name?returns=\(string\))       |
| Metis Testnet              | metis-goerli / 599            | [https://0xdeaddeaddeaddeaddeaddeaddeaddeaddead0000.metis-goerli.w3link.io/name?returns=(string)](https://0xdeaddeaddeaddeaddeaddeaddeaddeaddead0000.metis-goerli.w3link.io/name?returns=\(string\))             |
| Web3Q Galileo Testnet      | w3q-g / 3334                  | [https://qrobot.w3q.w3q-g.w3link.io/](https://qrobot.w3q.w3q-g.w3link.io/)                                                                                                                                       |
| BNB Smart Chain            | bnb / 56                      | [https://0xe9e7cea3dedca5984780bafc599bd69add087d56.bnb.w3link.io/name?returns=(string)](https://0xe9e7cea3dedca5984780bafc599bd69add087d56.bnb.w3link.io/name?returns=\(string\))                               |
| BNB Smart Chain Testnet    | bnbt / 97                     | [https://0xc5976c1ff6c550150293a31b5f9da787a3ebf5f0.bnbt.w3link.io/name?returns=(string)](https://0xc5976c1ff6c550150293a31b5f9da787a3ebf5f0.bnbt.w3link.io/name?returns=\(string\))                             |
| Avalanche C-Chain          | avax / 43114                  | [https://0xB31f66AA3C1e785363F0875A1B74E27b85FD66c7.avax.w3link.io/name?returns=(string)](https://0xb31f66aa3c1e785363f0875a1b74e27b85fd66c7.avax.w3link.io/name?returns=\(string\))                             |
| Avalanche Fuji Testnet     | fuji / 43113                  | [https://0x2796BAED33862664c08B8Ee5Fa2D1283C79593b1.fuji.w3link.io/name?returns=(string)](https://0x2796baed33862664c08b8ee5fa2d1283c79593b1.fuji.w3link.io/name?returns=\(string\))                             |
| Fantom Opera               | ftm / 250                     | [https://0x69c744d3444202d35a2783929a0f930f2fbb05ad.ftm.w3link.io/name?returns=(string)](https://0x69c744d3444202d35a2783929a0f930f2fbb05ad.ftm.w3link.io/name?returns=\(string\))                               |
| Fantom Testnet             | tftm / 4002                   | [https://0xf1277d1ed8ad466beddf92ef448a132661956621.tftm.w3link.io/name?returns=(string)](https://0xf1277d1ed8ad466beddf92ef448a132661956621.tftm.w3link.io/name?returns=\(string\))                             |
| Polygon Mainnet            | matic / 137                   | [https://0x0000000000000000000000000000000000001010.matic.w3link.io/name?returns=(string)](https://0x0000000000000000000000000000000000001010.matic.w3link.io/name?returns=\(string\))                           |
| Polygon Mumbai             | maticmum / 80001              | [https://0x431e9631cda6da17acb3ff3784df6cebed86b5f4.maticmum.w3link.io/name?returns=(string)](https://0x431e9631cda6da17acb3ff3784df6cebed86b5f4.maticmum.w3link.io/name?returns=\(string\))                     |
| Polygon zkEVM Testnet      | zkevmtest / 1402              | [https://0x6091F52B352Ea22a34d8a89812BA1f85D197F877.zkevmtest.w3link.io/name?returns=(string)](https://0x6091f52b352ea22a34d8a89812ba1f85d197f877.zkevmtest.w3link.io/name?returns=\(string\))                   |
| QuarkChain Mainnet Shard 0 | qkc-s0 / 100001               | [https://0xc2f21F8F573Ab93477E23c4aBB363e66AE11Bac5.qkc-s0.w3link.io/greet?returns=(string)](https://0xc2f21f8f573ab93477e23c4abb363e66ae11bac5.qkc-s0.w3link.io/greet?returns=\(string\))                       |
| QuarkChain Devnet Shard 0  | qkc-d-s0 / 110001             | [https://0xF2Fa1B7C11c33BAC1dB7b037478453289AC90E60.qkc-d-s0.w3link.io/greet?returns=(string)](https://0xf2fa1b7c11c33bac1db7b037478453289ac90e60.qkc-d-s0.w3link.io/greet?returns=\(string\))                   |
| Harmony Mainnet Shard 0    | hmy-s0 / 1666600000           | [https://0xcF664087a5bB0237a0BAd6742852ec6c8d69A27a.hmy-s0.w3link.io/name?returns=(string)](https://0xcf664087a5bb0237a0bad6742852ec6c8d69a27a.hmy-s0.w3link.io/name?returns=\(string\))                         |
| Harmony Testnet Shard 0    | hmy-b-s0 / 1666700000         | [https://0xc9c8ba8c7e2eaf43e84330db08915a8106d7bd74.hmy-b-s0.w3link.io/name?returns=(string)](https://0xc9c8ba8c7e2eaf43e84330db08915a8106d7bd74.hmy-b-s0.w3link.io/name?returns=\(string\))                     |
| Evmos                      | evmos / 9001                  | [https://0xc5e00d3b04563950941f7137b5afa3a534f0d6d6.evmos.w3link.io/name?returns=(string)](https://0xc5e00d3b04563950941f7137b5afa3a534f0d6d6.evmos.w3link.io/name?returns=\(string\))                           |
| Evmos Testnet              | evmos-testnet / 9000          | [https://0xae95d4890bf4471501e0066b6c6244e1caaee791.evmos-testnet.w3link.io/name?returns=(string)](https://0xae95d4890bf4471501e0066b6c6244e1caaee791.evmos-testnet.w3link.io/name?returns=\(string\))           |


## w3eth.io single-blockchain public gateway

Provided by the [EthStorage](https://eth-store.w3eth.io) team, w3eth.io is a single-blockchain public gateway for Ethereum mainnet and ENS. It supports the following formats:


* ``https://<domainName>.w3eth.io``
* ``https://<contractAddress>.w3eth.io``

Several examples:

* [https://cyberbrokers-meta.w3eth.io/renderBroker/5](https://cyberbrokers-meta.w3eth.io/renderBroker/5)
* [https://0x79a7aa92314fda49262649c6aef543fb0a652243.w3eth.io/render/78/0](https://0x79a7aa92314fda49262649c6aef543fb0a652243.w3eth.io/render/78/0)
* [https://vitalikblog.w3eth.io/](https://vitalikblog.w3eth.io/)