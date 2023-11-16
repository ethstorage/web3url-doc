# Background

The recent growth of decentralized applications such as decentralized finance (DeFi), non-fungible token (NFT), GameFi has dramatically created attention and discussion of the next-generation worldwide web, namely, Web3. Compared to the current Web2, which is controlled by a few centralized companies such as Facebook, Google, one key feature of Web3 is decentralized, which offers several unique benefits:

* Censorship-resistant, where all the assets are held by the user’s non-custodian wallet and therefore are not able to be frozen/confiscated assets by others.
* Transparency, where the users’ data are all on-chain so that everyone can access and use the data instead of monopolizing the user data by the centralized company in Web2.

To enjoy these benefits, the users have to access a Web3 infra, generally, a blockchain node, to read the data from or write the user data by signing a transaction and then submitting it to the blockchain P2P network. To lower the barrier of accessing the blockchain services for users without setting up a blockchain node, multiple wallets (such as MetaMask) and node service providers (NSPs, such as infura/quicknode) are created to provide blockchain services. To interact with the blockchain, the users can simply do the following steps (taking MetaMask as the wallet for example):

![Typical Flow of Using a dApp](<../.gitbook/assets/image (18).png>)

1. The user downloads and runs MetaMask as a browser extension (one-time step)
2. The user opens the website of a dApp (e.g., [www.opensea.io](http://www.opensea.io/) or [www.uniswap.org),](http://www.uniswap.org\),/) and the website server will respond with the webpage of the dApp. To generate the webpage, the website server may connect to **an NSP** to query the data on-chain, where the NSP may further connect to a trusted blockchain node setup by the NSP to obtain the data.
3. The browser receives the website response and then renders the data to the user. During rendering, the browser may also connect to **an NSP** to query the data needed to display
4. When the user wants to submit a transaction during browsing, a confirmation will be promoted by **MetaMask**. After the confirmation, **MetaMask** will submit the transaction to **an NSP** that uses a trusted blockchain node to broadcast the transaction to the blockchain P2P network.
