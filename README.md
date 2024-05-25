# poly-i

## Overview
This project demonstrates the creation and deployment of an NFT collection on the Ethereum Testnet, with batch minting and transfer to the Polygon Mumbai network. It leverages tools such as DALL·E 2 or MidJourney for image generation, IPFS for storage, and Solidity for smart contract development.

## Tools Used
- **Unix Computer (MacOS or Linux)**
- **Solidity**
- **Metamask**
- **Web Browser**
- **DALL·E 2 or MidJourney**: For generating a 5-item NFT collection
- **pinata.cloud**: For storing NFT images on IPFS
- **Ethereum Testnet**: For initial NFT deployment
- **Polygon network**: For mapping and visualizing the NFT collection
- **Hardhat**: For scripting and deploying contracts

## Steps to Create and Deploy the NFT Collection

### 1. Generate a 5-item Collection
Use DALL·E 2 or MidJourney to generate five unique images. Save these images locally for uploading to IPFS.

### 2. Store Items on IPFS
- Create an account on [pinata.cloud](https://pinata.cloud/).
- Upload the generated images to IPFS using Pinata.
- Note the IPFS hash for each image, which will be used to reference the images in the smart contract.

### 3. Deploy an ERC721 or ERC1155 Contract to Testnet
- Write a Solidity smart contract for your NFT collection, ensuring it includes a `promptDescription` function to return the prompt used for image generation.
- Compile and deploy the contract to the Ethereum Testnet using Hardhat.

Example `promptDescription` function:
```solidity
function promptDescription(uint256 tokenId) public view returns (string memory) {
    return tokenIdToPrompt[tokenId];
}
```

### 4. Map Your NFT Collection Using Polygon Network Token Mapper
- (Optional but recommended) Use the Polygon network token mapper for better visualization of your NFT collection.

### 5. Batch Mint All NFTs Using a Hardhat Script
Create a Hardhat script to batch mint all NFTs. The `ERC721A` standard is recommended for optimized gas usage.

Example Hardhat script for batch minting:
```javascript
async function main() {
    const [deployer] = await ethers.getSigners();
    const NFTContract = await ethers.getContractFactory("YourNFTContract");
    const nft = await NFTContract.deploy();
    await nft.deployed();

    const tokenURIs = [
        "ipfs://QmExampleHash1",
        "ipfs://QmExampleHash2",
        // Add the remaining IPFS hashes
    ];

    for (let i = 0; i < tokenURIs.length; i++) {
        await nft.safeMint(deployer.address, tokenURIs[i]);
    }
}

main().catch((error) => {
    console.error(error);
    process.exit(1);
});
```

### 6. Batch Transfer NFTs to Polygon Mumbai Using FxPortal Bridge
Write a Hardhat script to approve, deposit, and transfer NFTs to the Polygon Mumbai network using the FxPortal Bridge.

Example script steps:
1. **Approve NFTs** for transfer:
    ```javascript
    await nft.setApprovalForAll(fxRoot, true);
    ```

2. **Deposit NFTs** to the FxPortal Bridge:
    ```javascript
    await fxRoot.deposit(nft.address, deployer.address, tokenId, { gasLimit: 300000 });
    ```

### 7. Test `balanceOf` on Mumbai
After transferring the NFTs, verify the balances on the Mumbai network by testing the `balanceOf` function.

## Additional Scripts and Configuration
- Ensure your Hardhat configuration includes the Goerli and Mumbai network settings.
- Add any necessary scripts to automate the full process, including minting and transferring NFTs.

### Example Hardhat Config
```javascript
module.exports = {
    networks: {
        goerli: {
            url: process.env.GOERLI_URL,
            accounts: [process.env.PRIVATE_KEY]
        },
        mumbai: {
            url: process.env.MUMBAI_URL,
            accounts: [process.env.PRIVATE_KEY]
        }
    },
    solidity: "0.8.4",
};
```

## Conclusion
By following these steps, you can successfully create, deploy, and transfer an NFT collection from the Goerli Ethereum Testnet to the Polygon Mumbai network. This guide provides a comprehensive overview of the necessary tools and processes involved.
