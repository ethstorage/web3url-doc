# Upload Your First File with ethfs-sdk

## **Introduction**

In this tutorial, we will demonstrate how to upload files using the **ethfs-sdk** tool.&#x20;

## Step 1: Install ethfs-sdk

You can install ethfs-sdk by the following command

`npm i ethstorage-sdk`

or

`yarn add ethfs-sdk`

The npm page of ethfs-sdk can be found [here](https://www.npmjs.com/package/ethfs-sdk).

## Step 2: Create the FlatDirectory Contract

We want to create a [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) with the ethfs-sdk.

#### Browser

```
import { createDirectory } from "ethfs-sdk";
import { ethers } from "ethers";

const create = async () => {
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const signer = provider.getSigner();
    const directoryAddress = await createDirectory(signer);
    // directoryAddress : 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

#### Node.js

```
const ethfs = require("ethfs-sdk");
const ethers = require("ethers");

const create = async () => {
    const rpc = "https://galileo.web3q.io:8545";
    const privateKey = "0x...";
    const provider = new ethers.providers.JsonRpcProvider(rpc);
    const signer = new ethers.Wallet(privateKey, provider);
    const directoryAddress = await ethfs.createDirectory(signer);
    // directoryAddress : 0x37df32c7a3c30d352453dadacc838461d8629016
}
```

You will get a FlatDirectory address which is something like 0x...

## Step 3: Upload Files

In this section, you will upload some files into the FlatDirectory that you just created.

#### Browser

On the web page, you can use the input tag to get the file.

```
<input type="file" ref="avatar" :accept="accept"
       @change="onInputChange" />
       
const directoryAddress = "0x37df32c7a3c30d352453dadacc838461d8629016";

async function onInputChange (e) {
       // e.target.files is pseudo array, need to convert to real array
       const rawFiles = Array.from(e.target.files);
       for (const rawFile of rawFiles) {
           await uploadFile(directoryAddress, rawFile);
       }
}
```

Use ethfs-sdk to upload files.

```
import {upload} from "ethfs-sdk";
import {ethers} from "ethers";

const readFile = (file) => {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = (res) => {
      resolve(Buffer.from(res.target.result));
    };
    reader.readAsArrayBuffer(file);
  });
}

// callback, can be null
const onProgress = (chunkIndex, totalChunk, fileName) => {
    ...
}
const onSuccess = (fileName) => {
    ...
}
const onError = (message) => {
    ...
}

const uploadFile = async (directoryAddress, rawFile) => {
    const directoryPath = rawFile.name;
    const fileSize = rawFile.size;
    const content = await readFile(rawFile);
    
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const signer = provider.getSigner();
 
    await upload(signer, directoryAddress, directoryPath, fileSize, content,
        onProgress, onSuccess, onError);
}
```

#### Node.js

Node.js can read files using the **fs** module.

```
const ethfs = require("ethfs-sdk");
const ethers = require("ethers");
const fs = require('fs');

// callback, can be null
const onProgress = (chunkIndex, totalChunk, fileName) => {
    ...
}
const onSuccess = (fileName) => {
    ...
}
const onError = (message) => {
    ...
}

const uploadFile = async (directoryAddress, filePath, fileName) => {
    const fileStat = fs.statSync(filePath);
    const content = fs.readFileSync(filePath);
    const directoryPath = "img/" + fileName;
    
    const rpc = "https://galileo.web3q.io:8545";
    const privateKey = "0x...";
    const provider = new ethers.providers.JsonRpcProvider(rpc);
    const signer = new ethers.Wallet(privateKey, provider);
 
    await ethfs.upload(signer, directoryAddress, directoryPath, fileStat.size, content, 
        onProgress, onSuccess, onError);
}
```

## Step 4: Read Your File!

Now, you should be able to access the file you just uploaded via

`https://${address}.w3q-g.w3link.io/${directoryPath}`

Our two file access addresses are:

[https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/hello.txt](https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/hello.txt)

[https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/img/1.jpeg](https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/img/1.jpeg)
