# ethereum_token_MintableToken
Create an Ethereum(ERC-20) token using open source contracts(open-zeppelin)
You should get an email from them or be redirected to a site with your API Key. The one we want specifically is from the Ropsten Network.

Test Ethereum Network (Ropsten)
https://ropsten.infura.io/API_KEY
Copy your API_KEY.

Almost there! Now let’s start working on our deployment. Let’s head back in our code.

First things first, let’s talk about security. Create a new file in your root directory called .env. Your file structure should now look like this:

etherem_token_tutorial
|___contracts
| |_____Migrations.sol
| |_____TestToken.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
|___.env**(new file)


Inside your .env file lets add some environmental variables (these are variables that you can access anywhere in your code directory)

//.env file
INFURA_API_KEY=API_KEY
MNENOMIC=MNEOMIC_FROM_METAMASK
First add your API_KEY you copied into the file.

Remember the Mneomic(seed words) from initializing Metamask chrome extension? We’re going to need that now to deploy the contracts from. If you downloaded or wrote down your Mneomic, now write them down in your .env file MNENOMIC=SOME KEY PHRASE YOU DONT WANT THE PUBLIC TO KNOW.

IMPORTANT***

We added a .env file!!! We need to add a .gitignore file now to avoid adding the .env to a public repository if you ever decide to make the code public!

Create a .gitignore file in the same directory as your .env. Now it should look like this:

etherem_token_tutorial
|___contracts
| |_____Migrations.sol
| |_____TestToken.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
|___.env
|___.gitignore**(newfile)
Inside your .gitignore file:

// .gitignore
node_modules/
build/
.env
We want to ignore node_modules/ because when we do npm install it will download packages from our package.json. We want to ignore buildbecause later on when we run a script, it will create that directory for us automatically. We also want to ignore .env because it has private information we don’t want to release to the public.

Great! Over in our terminal we need to add two more modules.

npm install --save dotenv truffle-hdwallet-provider openzeppelin-solidity
Since we’re putting in private information, we need a way to access those variables from .env, and the dotenv package will help us.

The second package, truffle-hdwallet-provider is a wallet enabled provider. Without this, we would need to download all the blocks or use a light wallet to make new transactions in the Ethereum network. With the wallet provider and Infura API. We can deploy instantly, also bypassing painful processes.

Over in the truffle.js in our root directory, we need to modify some configurations.

// truffle.js
require('dotenv').config();
const HDWalletProvider = require("truffle-hdwallet-provider");
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 7545,
      gas: 6500000,
      network_id: "5777"
    },
    ropsten: {
        provider: new HDWalletProvider(process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY),
        network_id: 3,
        gas: 4500000
    },
  }
};
The first line indicates we want to use the .env variables in this repo. Generally in most apps, you only need to require this once in the starting config file.

Most of this is boilerplate. Main section we want to focus on is the ropsten network.

ropsten: {
        provider: new HDWalletProvider(process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY),
        network_id: 3,
        gas: 4500000
    },
The provider is our network. In our case, we want to deploy our token into the Ropsten network. Using the HDWalletProvider we pass in two arguments, process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY. We access our .env variables by referencing process.env.VARIABLE_NAME_IN_ENV.

We set the network_id: 3 because that represents Ropsten. 1 is the main Ethereum net and 2 is an old testnet.

Lastly, we set gas: 4500000, which is why we needed the Ethereum originally. We use gas/ethereum any time we need to modify/add something in the Ethereum Network.

Alright, onto the last step before deployment!

Over in our migrations/2_deploy_contract.js, we need to make some modifications for our contract.

// 2_deploy_contract.js
const TestToken = artifacts.require("./TestToken.sol");
module.exports = function(deployer) {
  deployer.deploy(TestToken);
};
If you named your token contract file something else. You need to replace the TestToken.sol to whatever file you named it.

truffle compile
This should create a new folder in your directory:

etherem_token_tutorial
|___build
| |_____contracts
|    |_____BasicToken.json
|    |_____ERC20.json
|    |_____ERC20Basic.json
|    |_____Migrations.json
|    |_____MintableToken.json
|    |_____Ownable.json
|    |_____SafeMath.json
|    |_____StandardToken.json
|    |_____TestToken.json
|___contracts
| |_____Migrations.sol
| |_____TestToken.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
|___.env
|___.gitignore**(newfile)
In our build folder, we have a bunch of contracts we inherited from the Open-Zeppelin library. If you’d like to know more about ERC-20 standards I’d check out the wiki. If there’s enough people asking for it I can make another blog post on it. For now here’s the link to the wiki.

Here comes the moment of truth. Now we need to deploy the contracts into the Ropsten network. Enter the following line in your terminal:

truffle migrate --network ropsten

This will take some time.
Reference to medium article here: https://medium.com/@danieljoonlee/create-an-ethereum-token-using-open-source-contracts-open-zeppelin-1e132e6233ed
