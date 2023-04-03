# Bind Domain Names to Your FlatDirectory

In the previous section, your website has been uploaded to a FlatDirectory contract and users can visit it through a web3 URL that contains the FlatDirectory contract address. What if we want to use a more user-friendly domain name instead of a contract address in the URL? Sure, we now support two domain services: ENS and W3NS, and you can bind any EVM-compatible contract address to the name!

## ENS Support

ENS is the most widely used name service in crypto, but interacting with a smart contract on ETH is super expensive (especially uploading a large amount of data), so it will be great to bind an ENS name to an [EIP-3770](https://eips.ethereum.org/EIPS/eip-3770) Address (a chain-specific address standard), and users can upload data on a cheaper chain/layer2 and still be able to use their ENS name to point to that.

As ENS users, you can add a new text record named “web3” (the value is an EIP-3770 address) in the ENS setting panel, and the Web3URL gateway will handle the rest of it.

<figure><img src="../.gitbook/assets/Screen Shot 2022-11-24 at 14.42.35.png" alt=""><figcaption></figcaption></figure>

Check out [https://w3url.w3eth.io](https://w3url.w3eth.io) and its [ENS setting page.](https://app.ens.domains/name/eth-store.eth/details)&#x20;

## W3NS Support

W3NS is a domain name service built on our Galileo testnet. If you don't have an ENS name and still want to use a domain name in your web3 URL, you can register a name in W3NS by following this [guide](register-your-first-w3ns-name.md).

After you get a W3NS name, you also need to set your FlatDirectory contract address in the "Web handler" field in the W3NS setting panel.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
