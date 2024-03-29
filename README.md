# Ethereum-Blockchain-for-Healthcare

## Prerequisits

We have used **Ubuntu 18.04.5 (64bit)** in a Virtual Machine (VirtualBox for example) to build this private network.

First, we will have to install Go Implementation of Ethereum, named "Geth". 

To do this, we will add a PPA (Personal Package Archives) to our apt repositories:
```
~$ sudo add-apt-repository -y ppa:ethereum/ethereum
```
Then we can install stable versions of Go Ethereum:
```
~$ sudo apt update
~$ sudo apt install ethereum
```
To verify if it has been done correctly, you can type:
```
~$ geth version
Geth
Version: 1.10.8-stable
Git Commit: 26675454bf93bf904be7a43cce6b3f550115ff90
Architecture: amd64
Go Version: go1.16.4
Operating System: linux
GOPATH=...
GOROOT=...
```
We can now start the setup for the ethereum network.

## Setup Scripts

We have made some setup scripts that can help you setup the ethereum node:

**setup.sh:**
This script will setup the node

You start by giving the name of the new node (or account):
```
Name of node to create/manage: node1
```
It will the start geth to create the account if it does not exist

After the account creation, you will be able to copy and past the Account public key and password you have chosen:
```
Remember to save you account public address key and password to a file
Account public key: 0xe3440e02675AA02B06F92ed0a05D4ABD2988E57E
Saved at ./accounts.txt

Password: 
Saved at ./node1/password.txt
```
Then, you will be invited to enter a Genesis file:
```
Genesis file (press enter to start the genesis file configuration): 
```
If you didn't create the genesis file beforhand, you will be able to start puppeth in order to create/configure the genesis file (see the section **Create a Genesis file:** for more information on how to do it)
```
Do tou want to start puppeth [Y/N]?
```
If you did create the genesis file, you can enter it here, and the programm will then initialise the node with the given genesis file and create the start script which will start the console for the node. A few information are needed:
```
Creation of the console command for this node, stored in the 'START_node1' script:
Console port (default=3010): 
Networkid (default=7410): 
RPC port (default=8520): 
ip address (default=127.0.0.1): 
```
The network id is the id you have chosen for your Blockchain. You can view it in the genesis file.

The Console port and RPC port can be whichever port you want, as long as they are not used. Note that if you chose to create another node on the same machine, you have to chose ports that weren't chosen before.

The ip address can be 127.0.0.1 (localhost) if you do not plan to connect nodes from another machine. If you want to connect nodes from another machine, the ip address will have to be a pingable addresse from the other machine.

## Ethereum node
Here, we will see how to setup an ethereum node. note that you have repeat this process for every peer before connecting them in a network.

First, start by creating a working environment (here, 'pnet' is already created):
```
~$ mkdir pnet
~$ cd pnet
pnet$ mkdir node1
```
Then we create an account (wallet). It will contain the user's private and public keys
```
pnet$ geth --datadir node1/ account new
INFO [09-17|10:34:07.533] Maximum peer count                   	ETH=50 LES=0 total=50
INFO [09-17|10:34:07.533] Smartcard socket not found, disabling	err="stat /run/pcscd/pcscd.comm: no such file or directory"
Your new account is locked with a password. Please give a password. Do not forget this password.
Password:
Repeat password:

Your new key was generated

Public address of the key:   0x4B39dA0C0914CFc3525A6CC8B5893d60032F9950
Path of the secret key file: node1/keystore/UTC--2021-09-05T15-47-02.743997096Z--4b39da0c0914cfc3525a6cc8b5893d60032f9950
```
Remember to save the puclic address of the key to a file, as well as the password in a text file, for example:
```
pnet$ echo '4B39dA0C0914CFc3525A6CC8B5893d60032F9950' >> accounts.txt
pnet$ echo '<password>' > node1/password.txt
```
**Create a Genesis file:**

The Genesis file is used to initialize the blockchain with a Genesis Block, based on the parameters contained in the genesis.json file used.

Puppeth helps create the genesis file:

Start puppeth :
```
pnet$ puppeth
```
Then enter a network name :
```
 Please specify a network name to administer (no spaces, hyphens or capital letters please)
> pnet
Sweet, you can set this via --network=pnet next time!
INFO [09-17|11:43:03.307] Administering Ethereum network       	name=pnet
WARN [09-17|11:43:03.367] No previous configurations found     	path=/home/pnet/.puppeth/pnet
```
After that, we will have to make some choices to create the genesis file from scratch. Here are the choices :
```
What would you like to do? (default = stats)
 1. Show network stats
 2. Configure new genesis
 3. Track new remote server
 4. Deploy network components
> 2
What would you like to do? (default = create)
 1. Create new genesis from scratch
 2. Import already existing genesis
> 1
Which consensus engine to use? (default = clique)
 1. Ethash - proof-of-work
 2. Clique - proof-of-authority
> 2
How many seconds should blocks take? (default = 15)
> 3
Which accounts are allowed to seal? (mandatory at least one)
> 0x95491f49d86d68ab43f55db5b6679b4d90f1ea84
> 0x
Which accounts should be pre-funded? (advisable at least one)
> 0x95491f49d86d68ab43f55db5b6679b4d90f1ea84
> 0x
Should the precompile-addresses (0x1 .. 0xff) be pre-funded with 1 wei? (advisable yes)
> yes
Specify your chain/network ID if you want an explicit one (default = random)
> 7410
INFO [09-20|10:07:39.675] Configured new genesis block
```
Then we can export your genesis configuration into a *.json* file
```
What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> 2
 1. Modify existing configurations
 2. Export genesis configurations
 3. Remove genesis configuration
> 2
Which folder to save the genesis specs into? (default = current)
  Will create pnet.json, pnet-aleth.json, pnet-harmony.json, pnet-parity.json
>
INFO [09-20|10:07:58.879] Saved native genesis chain spec      	path=pnet.json
ERROR[09-20|10:07:58.879] Failed to create Aleth chain spec    	err="unsupported consensus engine"
ERROR[09-20|10:07:58.915] Failed to create Parity chain spec   	err="unsupported consensus engine"
INFO [09-20|10:07:58.916] Saved genesis chain spec             	client=harmony path=pnet-harmony.json
What would you like to do? (default = stats)
 1. Show network stats
 2. Manage existing genesis
 3. Track new remote server
 4. Deploy network components
> ^C // Ctrl+c to exit
```
Once the genesis file is created, we have to edit the file and modify a parameter so blocks will only be created when there is a new transaction 
```
"period": 3 -> "period": 0,
```
**Initialize the node :**

Now we can initialize the node with the file we juste have created.
```
pnet$ geth --datadir node1/ init pnet.json
```
**Start the geth console :**

Now that everything is set up, we can start the geth console :
```
geth --port 3010 --networkid 7410 --datadir=./node1 --syncmode "full" --maxpeers=50  --rpc --rpcport 8520 --rpcaddr <IP_address> --rpccorsdomain "*" --rpcapi "eth,net,web3,personal,miner" --allow-insecure-unlock console 2>> eth.log
```
The console should look like this
```
instance: Geth/v1.9.3-stable-cfbb969d/linux-amd64/go1.11.5
coinbase: 0x95491f49d86d68ab43f55db5b6679b4d90f1ea84
at block: 0 (Fri, 20 Sep 2019 10:06:59 IST)
datadir: /home/cybrosys/Work/devnet/node1
modules: admin:1.0 clique:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
```
**Here are several useful commands:**

Get the account address :
```
> eth.coinbase
"0x95491f49d86d68ab43f55db5b6679b4d90f1ea84"
```
Unlock the account (you need to unlock an account whenever you want to do an action with it) :
```
> personal.unlockAccount(<account_address>,”yourpassword”,<time_unlock>)
true
> personal.unlockAccount(eth.coinbase,"pswd",3000) or personal.unlockAccount(eth.accounts[1],”yourpassword”,3000)
```
Create a new account on the node :
```
> personal.newAccount(“yourpassword”)
"0x99316969752a421b5ddc6e04b17274c2fd0d22a7"
```
Check the balance :
```
> eth.getBalance(<account_address>) 
9.046256971665327767466483203803742801036717552003169065582623750618213253
```
Make a fund transaction (A miner has to be active in order to valid the transaction) : 
```
> web3.eth.sendTransaction({from:eth.coinbase,to:eth.accounts[1],value:web3.toWei(300,"ether")}) "0xfce0e4b502691995c025c7980ac8a1b77592b96fd474e2bc6ee51a6504716fb3"
```
Start mining :
```
> miner.start() 
null
```
Open the log file (not in the geth console) :
```
$ tail -f eth.log
```
Load a script file :
```
> loadScript('<script.js>')
```
## Ethereum Peer Connection

**How to connect Multiple node to the POA network:**

First we have to do everything that we previously did with first node then we can add peers using the geth console on the node 2 :
```
admin.addPeer('enode://<node1_account_address>@<node1_IP_address>:3010') // true
```
We can check that nodes are connected with this command in the geth console of node 1 :
```
admin.peers
```

**Static conections between nodes:**

You can also make static connections between nodes. When it is done, the node will automatically try to connect to the nodes given in the file (by their enode).

To do this, go in the folder `pnet/node1/geth` and add (or edit) the file `static-nodes.json`. Here is the format of the file:
```
[
 "enode://<node2_account_address>@<node2_IP_address>:3010"
]
```
You can do the same thing for the trusted nodes, by adding the file `trusted-nodes.json`. The format is the same as `static-nodes.json and has to be placed in the same folder (`pnet/node1/geth`)

## Smart contract deployment

**1. Install solc (needed to compile the smart contract)**
```
$ sudo apt-get install -y solc
```
Check installation :
```
solc --version and geth version
```
**2. Write a smart contract**

First we need to create a .sol file. An example can be find at : https://medium.com/@soulmachine/develop-smart-contracts-using-solc-and-geth-f811ba917fca

**3. Compile a smart contract**

To compile the smart contract we just need to run the following command, the compilation is handle by geth.
```
$ echo "var storageOutput=`solc --optimize --combined-json abi,bin,interface Storage.sol`" > storage.js
```
Once it's done, launch the geth console and use this command :
```
> loadScript('storage.js')
```
We need some variables to be able to interact with the smart contract :
```
> var storageContractAbi = storageOutput.contracts['Storage.sol:Storage'].abi 
> var storageContract = eth.contract(JSON.parse(storageContractAbi))
> var storageBinCode = "0x" + storageOutput.contracts['Storage.sol:Storage'].bin
```
Note : If the second command doesn't work, try without «JSON.parse».

**4. Deploy**

First, we need to unlock the account, then we can deploy the smart contract :
```
> var deployTransationObject = {from: eth.accounts[0], data: storageBinCode, gas: 1000000 }; 
> var storageInstance = storageContract.new(deployTransationObject)
```
Now we need the smart contract address (wait a little for the transaction to be mined) :
```
> eth.getTransactionReceipt(storageInstance.transactionHash)
```
```
> var storageAddress = eth.getTransactionReceipt(storageInstance.transactionHash).contractAddress;
> var storage = storageContract.at(storageAddress);
```
Now we can interact with the smart contract:
```
> storage.get.call()
0 
> storage.set.sendTransaction(42, {from: eth.accounts[0], gas: 1000000})
"0x7a54ab329fcbf551432eb78c4b2a1ff48fc8b9f9aa23d94fa86330e5c1d711f3"
> storage.get.call()
42
```
