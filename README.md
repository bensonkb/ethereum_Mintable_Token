
We’ll be using Solidity to create our Ethereum token. But don’t worry,
there are a lot of open source libraries and contracts to help us in the
process.

What we want is an **ERC-20** compliant token. What that means is that
the Ethereum developers have decided a set of functionalities that is
necessary for your most common token usages today. There are other types
of ERC standards, but we wont dive into it.

**Requirements:**

-   Github
-   Terminal
-   NodeJS
-   NPM
-   Metamask (For initial Account Creation)

Alright let’s start coding! The first thing we want to do is download
`truffle`{.markup--code .markup--p-code}globally. You can visit their
repo at [truffle](https://github.com/trufflesuite/truffle) and here’s
the following snippet to install:

``` {#d1d1 .graf .graf--pre .graf-after--p name="d1d1"}
npm install -g truffle
```

**\*note**: make sure you have the latest version of truffle if you
installed this prior

Truffle will handle the smart contract compilation, linking, and
deployment for us. It’s a library that will make our lives easier for
this demonstration.

Now we need to create a directory where our project will live. In my
case I called it **ethereum\_token\_tutorial.**

So we have two options here. Either you can clone the repo I have
created by following this:

``` {#e219 .graf .graf--pre .graf-after--p name="e219"}
git clone -b initial_step https://git@github.com/danieljoonlee/ethereum_token_tutorial.git
```

Or you can do this in your terminal inside of your new directory:

``` {#7d3b .graf .graf--pre .graf-after--p name="7d3b"}
truffle init
```

If you followed the second option of doing `truffle init`{.markup--code
.markup--p-code}. The directory should look like this:

``` {#09e5 .graf .graf--pre .graf-after--p name="09e5"}
etherem_token_tutorial
|___contracts
| |_____ConvertLib.sol
| |_____MetaCoin.sol
| |_____Migrations.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
| |_____TestMetacoin.sol
| |_____metacoin.js
|___truffle.js
```

Go ahead and delete `ConvertLib.sol`{.markup--code .markup--p-code} ,
`MetaCoin.sol`{.markup--code .markup--p-code} ,
`TestMetacoin.sol`{.markup--code .markup--p-code} ,
`metacoin.js`{.markup--code .markup--p-code}.

So your directory should look like this now:

``` {#79c1 .graf .graf--pre .graf-after--p name="79c1"}
etherem_token_tutorial
|___contracts
| |_____Migrations.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
```

Great. Now we’re moving. Truffle helps us compile smart contracts and
deploy them. But we deleted our smart contract files other than the
migrating helper. Don’t worry, this is where Open-Zeppelin comes in.

`Open-Zeppelin`{.markup--code .markup--p-code} is an open source repo
where you can find smart contracts with generally best practices, good
test coverage, and most likely audited\*.

-   *Audit is when you have professional developers review your smart
    contracts looking for any leaks, bugs, or possibilities of malicious
    attacks.*

Here’s a link if you’re interested in smart contract attacks:
[Link](https://medium.com/@merunasgrincalaitis/how-to-audit-a-smart-contract-most-dangerous-attacks-in-solidity-ae402a7e7868)

For us to use any `Open-Zeppelin`{.markup--code .markup--p-code}
contracts we need to install it into our repository:

``` {#e5e5 .graf .graf--pre .graf-after--p name="e5e5"}
npm init -y
npm install -E zeppelin-solidity
```

We initialized a package.json with npm init -y. We also installed the
package for using the Open-Zeppelin contracts.

Okay, we’re going to write some Solidity. I did mention in the article
earlier that this will not be much code and I wasn’t joking!

Create a new file in the `contracts`{.markup--code .markup--p-code}
folder. In my case I named it `TestToken.sol`{.markup--code
.markup--p-code}

Now your directory should look like this:

``` {#a4e0 .graf .graf--pre .graf-after--p name="a4e0"}
etherem_token_tutorial
|___contracts
| |_____Migrations.sol
| |_____TestToken.sol***(this one is new)
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
```

In `TestToken.sol`{.markup--code .markup--p-code} we need to have the
following code:

``` {#eef6 .graf .graf--pre .graf-after--p name="eef6"}
// TestToken.solpragma solidity ^0.4.18;
```

``` {#7f23 .graf .graf--pre .graf-after--pre name="7f23"}
import "zeppelin-solidity/contracts/token/ERC20/MintableToken.sol";
```

``` {#0ec6 .graf .graf--pre .graf-after--pre name="0ec6"}
contract TestToken is MintableToken {    string public constant name = "Test Token";    string public constant symbol = "TT";    uint8 public constant decimals = 18;}
```

Let’s break this down since it’s quite a bit , even though it’s only a
few lines of code.

`pragma solidity ^0.4.18;`{.markup--code .markup--p-code}

It is required at the top of the file because it specifies the version
of Solidity we’re using.

``` {#e55c .graf .graf--pre .graf-after--p name="e55c"}
import "zeppelin-solidity/contracts/token/ERC20/MintableToken.sol";
```

The above code snippet is why Open-Zeppelin is so useful. If you know
how inheritance works, our contract is inheriting from MintableToken. If
you don’t know how inheritance works, MintableToken has a lot of
functionalities saved in inMintableToken.sol. We can use these
functionalities to create our token. If you visit this
[MintableToken](https://github.com/OpenZeppelin/zeppelin-solidity/blob/master/contracts/token/ERC20/MintableToken.sol)
you’ll notice a ton of functions and even more inheritance. It can be a
bit of a rabbit hole, but for this demonstration purpose, I want us to
release a token into the testnet.

For us, Mintable let’s us create as many tokens as we want, so we won’t
be starting with an initial supply. In my next article, we’ll create a
nodejs service that will create new tokens, and handle other ERC-20
standard functionalities.

The next bit of code:

``` {#7e56 .graf .graf--pre .graf-after--p name="7e56"}
contract TestToken is MintableToken {    string public constant name = "Test Token";    string public constant symbol = "TT";    uint8 public constant decimals = 18;}
```

This is where we can customize the token. In my case, I named mine “Test
Token”, with the symbol “TT”, and decimals of 18. But why 18 decimals?

Decimals of 18 is fairly standard in the community. So if we have one
test token it can potentially look like this 1.111111111111111111.

Whelp. That’s all the Solidity coding we need to do for this token. We
inherit all the main functionalities for a standardized ERC 20 token
from Open-Zeppelin. After that we need to set our constants for the
name, symbol, and decimals.

Before we forget, we should create a Metamask account and get it funded
with testnet ethereum.

Go ahead and search `MetaMask`{.markup--code .markup--p-code} extension
for Chrome, or follow this
[link](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/1EP3L66c3sIVd_S1OtqDPDQ.png)

Metamask Extension for Google Chrome

After you install MetaMask you should see a series of steps. You can
read through like terms of service. Eventually you’ll reach:

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/1UgnHq2z5aerWGDrHQzwCDg.png)

Metamask Password Screen

Input your password and confirm that password. On clicking create, you
will see another screen.

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/1gIdV2wxMDkoZigeBPEzikg.png)

Metamask secret

Make sure to save your seed words or copy them down into a text file. We
will need those seed words to deploy the token onto the testnet.

Also more **important**is to change your test from Mainnet Test Net to
Ropsten Test net. It’s on the top left of your MetaMask tab. Here is the
drop down:

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/11fzKT5SB740UlLE6Rmgdtw.png)

Testnet list

The reason we’re using Ropsten Test Network is because it’s the closest
testnet/implementation of the Main Ethereum Network.

Next you will need to copy your address to clipboard from
the `...`{.markup--code .markup--p-code} menu like so:

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/1Igjb1X-ksP7MEX5pH73Eog_002.png)

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/1Igjb1X-ksP7MEX5pH73Eog.png)

![](https://cdn-images-1.medium.com/max/800/1*Igjb1X-ksP7MEX5pH73Eog.png)

Metamask Account Screen

You should have an address similar to this one copied to your clipboard:

``` {#579b .graf .graf--pre .graf-after--p name="579b"}
address: 0x8EeF4Fe428F8E56d2202170A0bEf62AAc93989dE
```

This is the address from which we’re going to deploy our token contract.
Now one thing you need to know about deploying contracts is that they
cost Ethereum, to be specific Gas. We’re going to need to get some
testnet Ethereum into our accounts.

Now that you have your address go to this Ropsten faucet link:

[**Ethernet Faucet**\
*Edit
description*faucet.ropsten.be](http://faucet.ropsten.be:3001/ "http://faucet.ropsten.be:3001/")[](http://faucet.ropsten.be:3001/)

Copy and paste your address and soon you should have 1 Ethereum in your
MetaMask wallet for your address.

![](Create%20an%20Ethereum%20token%20using%20open%20source%20contracts%20(open-zeppelin)_files/16pLkmpPGZxknchPWvt72Dw.png)

Account with 1 ethereum

Just one more thing before we start coding our deployment process! We’re
going to use a free API called `Infura.io`{.markup--code
.markup--p-code}:

[**Infura — Scalable Blockchain Infrastructure**\
*Secure, reliable, and scalable access to Ethereum APIs and IPFS
gateways.*infura.io](https://infura.io/ "https://infura.io/")[](https://infura.io/)

Sign up for their services. You should get an email from them or be
redirected to a site with your API Key. The one we want specifically is
from the Ropsten Network.

``` {#8fa6 .graf .graf--pre .graf-after--p name="8fa6"}
Test Ethereum Network (Ropsten)https://ropsten.infura.io/API_KEY
```

Copy your API\_KEY.

Almost there! Now let’s start working on our deployment. Let’s head back
in our code.

First things first, let’s talk about security. Create a new file in your
root directory called `.env`{.markup--code .markup--p-code}. Your file
structure should now look like this:

``` {#c206 .graf .graf--pre .graf-after--p name="c206"}
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
```

Inside your `.env`{.markup--code .markup--p-code} file lets add some
environmental variables (these are variables that you can access
anywhere in your code directory)

``` {#adf9 .graf .graf--pre .graf-after--p name="adf9"}
//.env fileINFURA_API_KEY=API_KEYMNENOMIC=MNEOMIC_FROM_METAMASK
```

First add your API\_KEY you copied into the file.

Remember the Mneomic(seed words) from initializing Metamask chrome
extension? We’re going to need that now to deploy the contracts from. If
you downloaded or wrote down your Mneomic, now write them down in
your `.env`{.markup--code .markup--p-code} file
`MNENOMIC=SOME KEY PHRASE YOU DONT WANT THE PUBLIC TO KNOW.`{.markup--code
.markup--p-code}

**IMPORTANT\*\*\***

We added a `.env`{.markup--code .markup--p-code} file!!! We need to add
a `.gitignore`{.markup--code .markup--p-code} file now to avoid adding
the `.env`{.markup--code .markup--p-code} to a public repository if you
ever decide to make the code public!

Create a `.gitignore`{.markup--code .markup--p-code} file in the same
directory as your `.env`{.markup--code .markup--p-code}. Now it should
look like this:

``` {#bb4f .graf .graf--pre .graf-after--p name="bb4f"}
etherem_token_tutorial
|___contracts| 
|_____Migrations.sol| 
|_____TestToken.sol
|___migrations
| |_____1_initial_migrations.js
| |_____2_deploy_contracts.js
|___test
|___truffle.js
|___.env
|___.gitignore**(newfile)
```

Inside your `.gitignore`{.markup--code .markup--p-code} file:

``` {#3658 .graf .graf--pre .graf-after--p name="3658"}
// .gitignorenode_modules/build/.env
```

We want to ignore `node_modules/`{.markup--code .markup--p-code} because
when we do `npm install`{.markup--code .markup--p-code} it will download
packages from our `package.json`{.markup--code .markup--p-code}. We want
to ignore `build`{.markup--code .markup--p-code}because later on when we
run a script, it will create that directory for us automatically. We
also want to ignore `.env`{.markup--code .markup--p-code} because it has
private information we don’t want to release to the public.

Great! Over in our terminal we need to add two more modules.

``` {#8ed6 .graf .graf--pre .graf-after--p name="8ed6"}
npm install --save dotenv truffle-hdwallet-provider
```

Since we’re putting in private information, we need a way to access
those variables from `.env`{.markup--code .markup--p-code}, and the
`dotenv`{.markup--code .markup--p-code} package will help us.

The second package, truffle-hdwallet-provider is a wallet enabled
provider. Without this, we would need to download all the blocks or use
a light wallet to make new transactions in the Ethereum network. With
the wallet provider and Infura API. We can deploy instantly, also
bypassing painful processes.

Over in the `truffle.js`{.markup--code .markup--p-code} in our root
directory, we need to modify some configurations.

``` {#da40 .graf .graf--pre .graf-after--p name="da40"}
// truffle.jsrequire('dotenv').config();const HDWalletProvider = require("truffle-hdwallet-provider");
```

``` {#872e .graf .graf--pre .graf-after--pre name="872e"}
module.exports = {  networks: {    development: {      host: "localhost",      port: 7545,      gas: 6500000,      network_id: "5777"    },    ropsten: {        provider: new HDWalletProvider(process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY),        network_id: 3,        gas: 4500000    },  }};
```

The first line indicates we want to use the `.env`{.markup--code
.markup--p-code} variables in this repo. Generally in most apps, you
only need to require this once in the starting config file.

Most of this is boilerplate. Main section we want to focus on is the
ropsten network.

``` {#28b5 .graf .graf--pre .graf-after--p name="28b5"}
ropsten: {        provider: new HDWalletProvider(process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY),        network_id: 3,        gas: 4500000    },
```

The provider is our network. In our case, we want to deploy our token
into the `Ropsten`{.markup--code .markup--p-code} network. Using the
`HDWalletProvider`{.markup--code .markup--p-code} we pass in two
arguments,
`process.env.MNENOMIC, "https://ropsten.infura.io/" + process.env.INFURA_API_KEY`{.markup--code
.markup--p-code}. We access our `.env`{.markup--code .markup--p-code}
variables by referencing
`process.env.VARIABLE_NAME_IN_ENV`{.markup--code .markup--p-code}.

We set the `network_id: 3`{.markup--code .markup--p-code} because that
represents Ropsten. `1`{.markup--code .markup--p-code} is the main
Ethereum net and `2`{.markup--code .markup--p-code} is an old testnet.

Lastly, we set `gas: 4500000`{.markup--code .markup--p-code}, which is
why we needed the Ethereum originally. We use
`gas/ethereum`{.markup--code .markup--p-code} any time we need to
modify/add something in the Ethereum Network.

Alright, onto the last step before deployment!

Over in our `migrations/2_deploy_contract.js`{.markup--code
.markup--p-code}, we need to make some modifications for our contract.

``` {#a2ff .graf .graf--pre .graf-after--p name="a2ff"}
// 2_deploy_contract.js
```

``` {#524d .graf .graf--pre .graf-after--pre name="524d"}
const TestToken = artifacts.require("./TestToken.sol");
```

``` {#666b .graf .graf--pre .graf-after--pre name="666b"}
module.exports = function(deployer) {  deployer.deploy(TestToken);};
```

If you named your token contract file something else. You need to
replace the `TestToken.sol`{.markup--code .markup--p-code} to whatever
file you named it.

``` {#a621 .graf .graf--pre .graf-after--p name="a621"}
truffle compile
```

This should create a new folder in your directory:

``` {#bc6f .graf .graf--pre .graf-after--p name="bc6f"}
etherem_token_tutorial|___build| |_____contracts|    |_____BasicToken.json|    |_____ERC20.json|    |_____ERC20Basic.json|    |_____Migrations.json|    |_____MintableToken.json|    |_____Ownable.json|    |_____SafeMath.json|    |_____StandardToken.json|    |_____TestToken.json|___contracts| |_____Migrations.sol| |_____TestToken.sol|___migrations| |_____1_initial_migrations.js| |_____2_deploy_contracts.js|___test|___truffle.js|___.env|___.gitignore**(newfile)
```

In our build folder, we have a bunch of contracts we inherited from the
Open-Zeppelin library. If you’d like to know more about ERC-20 standards
I’d check out the wiki. If there’s enough people asking for it I can
make another blog post on it. For now here’s the link to the
[wiki.](https://theethereum.wiki/w/index.php/ERC20_Token_Standard)

Here comes the moment of truth. Now we need to deploy the contracts into
the Ropsten network. Enter the following line in your terminal:

``` {#953b .graf .graf--pre .graf-after--p name="953b"}
truffle migrate --network ropsten
```

You should get a series of lines in your terminal like:

``` {#36b3 .graf .graf--pre .graf-after--p name="36b3"}
Using network 'ropsten'.
```

``` {#e2e1 .graf .graf--pre .graf-after--pre name="e2e1"}
Running migration: 1_initial_migration.js  Deploying Migrations...  ... 0x7494ee96ad7db4a560b6f3169e0666c3938f9f54208f7972ab902feb049a7f68  Migrations: 0x254466c5b09f141ce1f93689db6257b92133f51aSaving successful migration to network...  ... 0xd6bc06b3bce3d15dee4b733e5d4b09f0adb8f93f75ad980bad078484641d36e5Saving artifacts...Running migration: 2_deploy_contracts.js  Deploying TestToken...  ... 0x7e5c1b37f1e509aea59cd297417efe93eb49fdab2c72fa5c37dd2c63a3ba67b7  TestToken: 0x02ec6cbd89d3a435f8805e60e2703ef6d3147f96Saving successful migration to network...  ... 0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34eSaving artifacts...
```

This will take some time. Once it’s fully deployed copy the last txid.
In my case:

``` {#0d8b .graf .graf--pre .graf-after--p name="0d8b"}
0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e
```

This will have an address to your token contract. Here is a link to my
txid:

[**Ropsten Transaction
0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e**\
*Ropsten (ETH) detailed transaction info for
0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e*ropsten.etherscan.io](https://ropsten.etherscan.io/tx/0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e "https://ropsten.etherscan.io/tx/0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e")[](https://ropsten.etherscan.io/tx/0x2fd6d699295d371ffd24aed815a13c5a44e01b62ca7dc6c9c24e2014b088a34e)

Which has an address to the contract itself:

[**Ropsten Accounts, Address and Contracts**\
*The Ethereum BlockChain Explorer, API and Analytics
Platform*ropsten.etherscan.io](https://ropsten.etherscan.io/address/0x254466c5b09f141ce1f93689db6257b92133f51a "https://ropsten.etherscan.io/address/0x254466c5b09f141ce1f93689db6257b92133f51a")[](https://ropsten.etherscan.io/address/0x254466c5b09f141ce1f93689db6257b92133f51a)

You can get the completed github repo
[here](https://github.com/bensonkb/ethereum_Mintable_Token/).

This part one of a series of creating a token and interacting with it.
In the next blog we will create a simple node microservice. We will use
this service to call various functions on your token smart contract,
such as minting new tokens, transferring, etc.

