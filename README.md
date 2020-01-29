# RSK-Tutorial
Build a basic RSK Dapp in 2 hours.
Uses Javascript and HTML
Uses Truffle (or Remix)
Uses Metamask 
Uses ethersjs. 

### 1. Install metamask
* Use a fresh wallet you are not afraid to lose. 
* save your mnemonic (will need later)

* connect to RSK network
  on the top select custom RPC
    
  input the following details
  - other details
  - Network Name: RSK Testnet
  - RPC URL: https://public-node.testnet.rsk.co
  - chainID: 31 (optional)
  - symbol: RBTC (optional)
  - Block Explorer URL (optional): https://explorer.testnet.rsk.co
  
* Visit [the offical RSK faucet](https://faucet.testnet.rsk.co/). Acquire some testnet RBTC. 
 
### 2. Set up the truffle box (truffle option)
Let's copy a truffle box that comes with the pre-configurations needed for rsk.

* Clone the rsk-starter-pack
```bash
clone https://github.com/rsksmart/rsk-starter-box
```

* Install truffle if you haven't already. We will also need truffle/hdwallet provider
```bash
npm install -g truffle
npm install @truffle/hdwallet-provider
```

* Now we can use truffle to "unbox" the starter pack. 

```bash
truffle unbox rsksmart/rsk-starter-box
```

* Open the repository in your favorite code editor and take a quick look at the files. 

* Look inside truffle-config.js. You can see mnemonic variable and mainnet/testnet configurations. 

* replace the mnemonic variable with your mnemonic from earlier. 

### 3. Create HTML Web Page

* create files called index.html and index.js in a new directory

* create a basic html page

```
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    
  </body>
</html>
```

* add an input, label, and button to Get and Set a variable from the blockchain
```
<input id="dataToWrite" placeholder = "value to Set"></input> <button onclick="write()">Set</button> <br>
    		<button onclick="read()">Get</button> <label id="dataToShow"></label>
```

* That's it for the front end!

* in index.js create three variables and three functions: 
```
//Set up the connection to the RSK blockchain
let contract
let contractAddress
let contractABI
async function initialize(){
}
//Read a value from the smart contract
async function read(){
}
//Write a value to the smart contract
async function write() {
}
```

* These functions will be 

### 4. Write your smart contract. 

* Go to remix.ethereum.org, create a new file,  and copy this code into it.

```
pragma solidity 0.6.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```
* Make sure the code compiles in remix

* Take a moment to understand the code. There is a single variable, and two functions to get and set it's value.

* Feel free to play around with the solidity code and write your own version.

* Save the contract code in "SimpleStorage.sol" in the contracts folder of the truffle project

* Copy the ABI (in the compile tab on remix) and save it for later use. 

### 5a. Deploy the contract with truffle (truffle option)

* Compile your contract
```bash
truffle compile 
```
* Deploy your contract
```bash
truffle migrate --testnet 
```
* View the deployed contract on https://explorer.testnet.rsk.co. 



### 5b. Deploy the contract with remix (remix option)

* make sure your metamask is unlocked, and set the the RSK testnet

* add the "deploy and run transactions" plugin

* click Deploy on the "deploy and run transactions" tab.

* View the deployed contract on https://explorer.testnet.rsk.co. 

    
### 6. Connect your app to RSK and call smart contract

* Let's install ethersjs in your app by adding this script tag into index.html
```
  <script charset="utf-8"
        src="https://cdn.ethers.io/scripts/ethers-v4.min.js"
        type="text/javascript">
   </script>
```

This lets us use the ethersjs javascript library to interact with RSK. RSK is compatible with the Ethereum virtual machine so ethersjs works like a charm. 

* In index.js, define a new contract and contractAddress and contractABI. Use your deployed contract for the contract address (in quotes). and paste your contractABI
```
let contract
let contractAddress = 0x00YourContractAddress
let contractABI = [Your Contract ABI]
```

* write a new async function called initialize
```
async function initialize() {
//Define A provider, use the web3 connection provided by Metamask
	provider = new ethers.providers.Web3Provider(web3.currentProvider);
  
  //Define a wallet object, which you will use to interact with the contract
	let accounts = await provider.listAccounts()
	wallet = provider.getSigner(accounts[0])
	walletAddress = wallet._address;

//create a new contract object which connects to the contract you deployed, to be interacted with by your wallet. 
  contract = new ethers.Contract(contractAddress,contractABI,wallet)
	console.log(contract)
  }
```

* Read the code carefully before proceeding. Understand each line and study the comments. 

* In index.html, import index.js and call initialize when the page loads. The web3 object is injected automatically by Metamask. 
```
  <script src = index.js></script>
  <script>initialize(web3)</script>
```

* Write a get and set function for index.js. These will get and set the value in the smart contract

```
async function read(){

    document.getElementById("dataToShow").innerHTML = await contract.get();

}

async function write(){
  let data = document.getElementById("dataToWrite").value
  console.log(data)
  await contract.set(data)
}
```

* Again take time to understand the code before proceeding

* In index.html, 

### 7. Test it out!

* Navigate to the 'src' folder in your terminal and input the following command to start you server

```
python -m http.server
```
Navigate to http://localhost:8000/ in your browser and test your app out to see if the set and get buttons work as expected. 

### 8. Notes

* Don't forget to redo your ABI evertime you make changes to your code
*


