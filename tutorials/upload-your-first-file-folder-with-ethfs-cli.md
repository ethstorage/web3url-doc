# Upload Your First File/Folder with ethfs-cli

## **Introduction**

In this tutorial, we will demonstrate how to upload files or folders using the **ethfs-cli** tool. We assume that

* there is a folder to be uploaded; (dist)
* there are two files in the folder. (hello.txt and img/1.jpeg)

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Step 1: Install ethfs-cli

You can install ethfs-cli by the following command

`npm i -g ethfs-cli`

The npm page of ethfs-cli can be found [here](https://www.npmjs.com/package/ethfs-cli).

## Step 2: Create the FlatDirectory Contract

We want to create a [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) with the private key 0x112233... on the Sepolia

```
ethfs-cli create -p 0x112233... -c 11155111
```

If we want to create it on the other chains that support EVM, we need to change the chainId, such as the QuarkChain L2.

```
ethfs-cli create -p 0x112233... -c 110011
```

The RPC URL of the chain can also be specified.

```
ethfs-cli create -p 0x112233... -r https://...
```

You will get a FlatDirectory address: [<mark style="color:blue;">0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f</mark>](https://sepolia.etherscan.io/address/0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f) after the transaction is confirmed.


## Step 3: Upload Files

In this section, you will upload the folder into the FlatDirectory that you just created.

Run the command to upload the file.

```
ethfs-cli upload -a <address> -p <private-key> -f <directory|file> -c [chain-id] -t <upload-type> -e
```

The command to upload the contents of the "_dist"_ folder to the address "_0x5fF2...B6AD"_ is:

```
ethfs-cli upload -a 0x000B... -p 0x112233... -f /Users/.../dist -c 11155111 -t blob -e
```

The execution results are as follows.

```log
ℹ️ INFO:      Provider URL: https://rpc.sepolia.org
ℹ️ INFO:      Chain ID: 11155111
ℹ️ INFO:      Address: 0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f
ℹ️ INFO:      Thread pool size: 1

FlatDirectory: Transaction hash: 0x9247464926c2f110b4c79a0d03e19e140b49476a31b948ae630bc9762eb8f951 for chunk(s) 0. (Key: hello.txt)
FlatDirectory: Chunks 0 have been uploaded for hello.txt.
FlatDirectory: Transaction hash: 0x76cce4c327605c32841774e031e8090e5d5ea9f7cf46e70824df4385a30a0b53 for chunk(s) 0. (Key: img/1.jpeg)
FlatDirectory: Chunks 0 have been uploaded for img/1.jpeg.

✅  FINISH:    Total files: 2
✅  FINISH:    Total chunks uploaded: 3
✅  FINISH:    Total data uploaded: 22.953125 KB
✅  FINISH:    Total cost: 0.000178459740966161 ETH
```

## Step 4: Set Default File

You can also set the default file for FlatDirectory using the _default_ command.

Run the command to set the default file.

```
ethfs-cli default -a <address> -f <name> -p <private-key> -c <chain-id>
```

The command to set the default file "_hello.txt"_ for "_0x5fF2...B6AD_" is:

```
ethfs-cli default -a 0x000B -f hello.txt -p 0x1122.. -c 11155111
```

## Step 5: Browse Your File!

Now, you should be able to browse the files that are just uploaded via

`https://${address}.3333.w3link.io/${filename}`

Our two file access addresses are:

[https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/hello.txt](https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/hello.txt)

[https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/img/1.jpeg](https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/img/1.jpeg)

Because the default file has been set to "_hello.txt"_ above, you can access it through the following link.

[https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/](https://0x000B467A2eEe67fFef30922a1CE13F7D50Ad4C5f.3333.w3link.io/)

