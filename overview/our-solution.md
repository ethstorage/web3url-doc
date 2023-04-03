# Our Solution

We expect that as more and more users and developers face the current problems of Web3 (or Web2.5 more precisely?), we need a solution so that we could fully harvest the benefits of blockchain. That is why we believe the future of Web3 should be

> End-to-End Fully Trustless Decentralized Web

This means that any components in our Web2 from the user side and server side must be decentralized in Web3.

* We can still rely on TCP/IP protocol, which is already decentralized and used by the blockchain P2P network.
* We should not rely on the DNS protocol as it could still censor a translation of a domain. Instead, we could use **blockchain-based name services** such as ENS.
* We should not use the current client/webserver model, where the webserver itself is centralized. Instead, we could use a blockchain network where **the blockchain P2P network itself can serve as a webserver**!
* We do not want to change current users’ web experience, especially the HTTP protocol. This can be done by designing the blockchain network support HTTP protocol in a decentralized way (decentralized HTTP?)!

Achieving all these seem to be extremely challenging. The following graph illustrates one solution, where

1. The user installs a verified extension (like downloading geth from github), which serves as a light client to the blockchain P2P network.
2. When the user types a web3 URL (e.g., web3://xxxxx), the extension will parse the URL and translate it to a blockchain message (e.g., calling a smart contract). Then the extension will deliver the message to the P2P network and query the result. For any result returned from the network, the extension fully verified that the result is trusted.
3. The trusted result is returned to the web browser. The result will be mostly like an HTML document that may contain more web3 URLs.

![Fully Decentralized Web Solution](<../.gitbook/assets/Screen Shot 2022-04-11 at 11.50.21 AM.png>)

\
Our solution aims to replace the centralized choke points in web2 stack, as shown below:

![Components](<../.gitbook/assets/Screen Shot 2022-04-11 at 11.53.39 AM.png>)

1. A dedicated Ethereum sidechain which supports EVM and efficient binary large object (BLOB) storage will replace the traditional client / server model
2. A [web3 style URL standard](https://github.com/ethstorage/W3IPs/blob/main/W3IPS/w3ip-1.md) will replace the traditional DNS / URL scheme
3. Web browser extensions acting as light clients and performing EVM calls  from web3 URLs will be the decentralized HTTP

![Component Details](<../.gitbook/assets/Screen Shot 2022-04-11 at 11.52.42 AM.png>)
