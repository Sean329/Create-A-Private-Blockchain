# Create-A-Private-Blockchain
Create a blockchain application to register ownership for the stars discovered by Astronomy enthusiasts (Proof of Existence). It's built with Node.js. 
The main files are:
1. src/block.js
2. src/blockchain.js
3. * node_modules.zip needs to be unzipped.

# Testing The App
The project is build as an express.js app with endpoints that are defined in BlockchainController.js. It's suggested to use POSTMAN to test the application.

## getBlockByHeight
The response will show that there is no previous block at height 0 and establish a genesis block hash that will be used in the generation of the subsequent block's hash.

<img width="1779" alt="1  genesis block" src="https://user-images.githubusercontent.com/7294966/149120665-b742fb53-3efc-434d-a9ef-566130ad83d7.png">


## requestOwnership
This endpoint allows a user to generate a message to be signed with a Bitcoin wallet. The message follows the format WALLET_ADDRESS: TIME :starRegistry. The wallet address is the required input, and is also required to perform subsequent steps.

<img width="1781" alt="2  register for the 1st star" src="https://user-images.githubusercontent.com/7294966/149121620-b1d8a841-51ab-4bd3-8718-b76214879b10.png">


## submitStar
In this step, we submit a message signed by the Bitcoin wallet and put it on the private blockchain. Since the digital signature proves owership of the wallet address, this message is guaranteed to have been signed by the owner of the wallet. The content in "star" can be anything, but once added to the Blockchain it cannot be changed. The hashed signature cannot be altered too. If an attempt were made to change any data in this block, subsequent blocks would not validate. To submit a star, the msg in the following snapshots can be used as a model (note this signature while valid will not work because our blockchain will reject a message that is more than 5 minutes behind current time).

![3  sign message for the 1st star with my wallet](https://user-images.githubusercontent.com/7294966/149122839-dca7928f-d6a0-4489-8ca0-1c0328d2232e.png)

<img width="1779" alt="4  submit the 1st start with my signed message" src="https://user-images.githubusercontent.com/7294966/149122856-948b8470-1473-40e3-9078-d025982c1fbc.png">


## getStarsByWalletAddress
Next, we show that blocks can also be filtered by the wallet address that signed the block. To do this, we need to decode the encoded body of each block and check whether the address submitted in the endpoint request is a match.
The code to retrieve blocks by address wraps an async function that decodes each block and checks the address in a promise that is resolved with the blocks that match the provided address.

```
getStarsByWalletAddress (address) {
        let self = this;
        let stars = [];
        return new Promise(async (resolve, reject) => {
            for (let block of self.chain) {
                let blockData = await block.getBData();
                if (blockData.owner === address) stars.push(blockData)
            };
            resolve(stars);
        });
    }
```   

<img width="1779" alt="5  All my 3 stars" src="https://user-images.githubusercontent.com/7294966/149123760-12b9c700-4db0-47c2-ab24-37f47565583d.png">






