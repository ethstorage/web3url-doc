# Upload Your First File with ethstorage-sdk

## **Introduction**

In this tutorial, we will demonstrate how to upload files using the **ethstorage-sdk** tool.&#x20;

## Step 1: Install ethstorage-sdk

You can install ethstorage-sdk by the following command

`npm i ethstorage-sdk`

or

`yarn add ethstorage-sdk`

The npm page of ethstorage-sdk can be found [here](https://www.npmjs.com/package/ethstorage-sdk).

## Step 2: Create the FlatDirectory Contract

We want to create a [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) with the ethstorage-sdk.

```
import { FlatDirectory } from "ethstorage-sdk";

const rpc = "https://rpc.testnet.l2.quarkchain.io:8545";
const ethStorageRpc = "https://rpc.testnet.l2.ethstorage.io:9540";
const privateKey = "0xabcd...";

const flatDirectory = await FlatDirectory.create({
    rpc: rpc,
    ethStorageRpc: ethStorageRpc,
    privateKey: privateKey,
});

const contracAddress = await flatDirectory.deploy();
console.log(`FlatDirectory address is ${contracAddress}.`);
```

You will get a FlatDirectory address which is something like 0x...

## Step 3: Upload Files

In this section, you will upload some files into the FlatDirectory that you just created.

Callback

```
const callback = {
    onProgress: function (progress, count, isChange) {
        ...
    },
    onFail: function (err) {
        ...
    },
    onFinish: function (totalUploadChunks, totalUploadSize, totalStorageCost) {
        ...
    }
};
```

#### Browser

On the web page, you can use the input tag to get the file.

```
<input type="file" ref="avatar" :accept="accept"
       @change="onInputChange" />

async function onInputChange (e) {
       // e.target.files is pseudo array, need to convert to real array
       const rawFiles = Array.from(e.target.files);
       for (const rawFile of rawFiles) {
           await uploadFile(rawFile);
       }
}

async function uploadFile(rawFile) {
   await flatDirectory.upload({
        key: rawFile.name,
        content: rawFile,
        type: 2,
        callback: callback
    });
}
```

#### Node.js

Node.js can read files using the **NodeFile** module.

```
const { NodeFile } = require("ethstorage-sdk/file");

async function uploadFile(filePath, fileName) {
    const file = new NodeFile(filePath);
    await flatDirectory.upload({
        key: fileName,
        content: file,
        type: 2,
        callback: callback
    });
}
```

## Step 4: Read Your File!

Now, you should be able to access the file you just uploaded via

`https://${address}.3336.w3link.io/${directoryPath}`

Our two file access addresses are:

[https://0x336fbd105c07db2317149bae143e378ccdf56b3d.3336.w3link.io/hello.txt](https://0x336fbd105c07db2317149bae143e378ccdf56b3d.3336.w3link.io/hello.txt)

[https://0x336fbd105c07db2317149bae143e378ccdf56b3d.3336.w3link.io/img/1.jpeg](https://0x336fbd105c07db2317149bae143e378ccdf56b3d.3336.w3link.io/img/1.jpeg)
