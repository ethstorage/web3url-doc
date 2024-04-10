# Upload Your First File/Folder with ethfs-uploader

## **Introduction**

In this tutorial, we will demonstrate how to upload files or folders using the **ethfs-uploader** tool. We assume that

* there is a folder to be uploaded; (dist)
* there are two files in the folder. (hello.txt and img/1.jpeg)

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Step 1: Install ethfs-uploader

You can install ethfs-uploader by the following command

`npm i ethfs-uploader`

The npm page of ethfs-uploader can be found [here](https://www.npmjs.com/package/ethfs-uploader).

## Step 2: Create the FlatDirectory Contract

We want to create a [FlatDirectory](https://docs.web3url.io/advanced-topics/flatdirectory) with the private key 0x112233... on the default Chain (Galileo Testnet)

```
npx ethfs-uploader --create --privateKey 0x112233...
```

If we want to create it on the other chains that support EVM, we need to add the chainId, such as the Goerli.

```
npx ethfs-uploader --create --privateKey 0x112233... --chainId 5
```

If the RPC URL of the chain can also be specified.

```
npx ethfs-uploader --create --privateKey 0x112233... --chainId 5 --RPC https://...
```

You will get a FlatDirectory address: [<mark style="color:blue;">0x37DF32c7a3c30D352453dadACc838461d8629016</mark>](https://explorer.galileo.web3q.io/address/0x37DF32c7a3c30D352453dadACc838461d8629016/transactions) after the transaction is confirmed.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

## Step 3: Upload Files

In this section, you will upload the folder into the FlatDirectory that you just created.

In order to identify which chain the FlatDirectory contract is on, the short name of the chain needs to be added before the contract address. See more details about [the EIP-3770 address](https://eips.ethereum.org/EIPS/eip-3770).

```
shortName:address
```

The address created above on the **Galileo** network are spliced ​​as follows.

```
w3q-g:0x37DF32c7a3c30D352453dadACc838461d8629016
```

Run the command to upload the file.

```
npx ethfs-uploader <directory|file> <address> --privateKey <private-key>
```

The command to upload the contents of the "_dist"_ folder to the address "_0x37D...9016"_ is:

```
npx ethfs-uploader /Users/.../dist w3q-g:0x37DF32c7... --privateKey 0x112233...
```

The execution results are as follows.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## Step 4: Set Default File

You can also set the default file for FlatDirectory using the _default_ command.

Run the command to set the default file.

```
npx ethfs-uploader --default --address <address> --file <name> --privateKey <private-key>
```

The command to set the default file "_hello.txt"_ for "_0x37D...9016_" is:

```
npx ethfs-uploader --default w3q-g:0x37D... --file hello.txt. --privateKey 0x1122..
```

## Step 5: Browse Your File!

Now, you should be able to browse the files that are just uploaded via

`https://${address}.w3q-g.w3link.io/${filename}`

Our two file access addresses are:

[https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/hello.txt](https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/hello.txt)

[https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/img/1.jpeg](https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/img/1.jpeg)

Because the default file has been set to "_hello.txt"_ above, you can access it through the following link.

[https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/](https://0x37df32c7a3c30d352453dadacc838461d8629016.w3q-g.w3link.io/hello.txt)

## Step 6: Read and Write FlatDirectory with js code

Get the abi of the FlatDirectory contract.

```
const flatDirectoryAbi = [
  "function write(bytes memory name, bytes memory data) external payable",
  "function read(bytes memory name) external view returns (bytes memory, bool)",
  "function writeChunk(bytes memory name, uint256 chunkId, bytes memory data) external payable",
  "function readChunk(bytes memory name, uint256 chunkId) external view returns (bytes memory, bool)"
];
```

Create a contract object using [ethers](https://docs.ethers.org/v5/).

```
export const FlatDirectoryContract = (address) => {
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const contract = new ethers.Contract(address, flatDirectoryAbi, provider);
    return contract.connect(provider.getSigner());
};
```

Read file

<pre><code>const contract = FlatDirectoryContract('0x37DF32c7a3c30D352453dadACc838461d8629016');
const hexName = '0x' + Buffer.from('hello.txt', 'utf8').toString('hex');
<strong>const content = contract.read(hexName);
</strong>console.log(content);
</code></pre>

Write file

```
const filePath = '';
let fileSize = ;
const hexName = '0x' + Buffer.from('img/1.jpeg', 'utf8').toString('hex');
const contract = FlatDirectoryContract('0x37DF32c7a3c30D352453dadACc838461d8629016');
const content = fs.readFileSync(filePath);

// Data need to be sliced if file > 475K
let chunks = [];
if (fileSize > 475 * 1024) {
  const chunkSize = Math.ceil(fileSize / (475 * 1024));
  chunks = bufferChunk(content, chunkSize);
  fileSize = fileSize / chunkSize;
} else {
  chunks.push(content);
}
// Files larger than 24k need stak tokens
let cost = 0;
if (fileSize > 24 * 1024 - 326) {
  cost = Math.floor((fileSize + 326) / 1024 / 24);
}

for (const index in chunks) {
  const chunk = chunks[index];
  const hexData = '0x' + chunk.toString('hex');

  const estimatedGas = await contract.estimateGas.writeChunk(hexName, index, hexData, {
    value: ethers.utils.parseEther(cost.toString())
  });
  // upload file
  const option = {
    gasLimit: estimatedGas.mul(6).div(5).toString(),
    value: ethers.utils.parseEther(cost.toString())
  };
  const tx = await fileContract.writeChunk(hexName, index, hexData, option);
  console.log(`Transaction Id: ${tx.hash}`);
  
  // get result
  const receipt = await tx.wait();
  if (receipt.status) {
    console.log(`File ${fileName} chunkId: ${index} uploaded!`);
  }
}
```
