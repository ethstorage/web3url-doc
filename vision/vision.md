---
description: Introduction of Web3URL
---

# Vision

> Web3URL is an access protocol for future web3

This is what we believe the future of Web3 should be.

In other words, every component in our Web2 from the user side and server side must be decentralized in Web3.

* We can still rely on TCP/IP protocol, which is already decentralized and used by the blockchain P2P network.
* We should not rely on the DNS protocol as it could still censor a translation of a domain. Instead, we could use **blockchain-based name services** such as ENS.
* We should not use the current client/webserver model, where the webserver itself is centralized. Instead, we could use a blockchain network where **the blockchain P2P network itself can serve as a webserver**!
* We do not want to change current users’ web experience, especially the HTTP protocol. This can be done by designing the blockchain network to support HTTP protocol in a decentralized way (decentralized HTTP?)!

Achieving all these seem to be extremely challenging. The following graph illustrates one solution, where

1. The user installs a verified extension (like downloading geth from github), which serves as a light client to the blockchain P2P network.
2. When the user types a web3 URL (e.g., web3://xxxxx), the extension will parse the URL and translate it to a blockchain message (e.g., calling a smart contract). Then the extension will deliver the message to the P2P network and query the result. For any result returned from the network, the extension fully verified that the result is trusted.
3. The trusted result is returned to the web browser. The result will be mostly like an HTML document that may contain more web3 URLs.

![Fully Decentralized Web Solution](<../.gitbook/assets/image (4).png>)
