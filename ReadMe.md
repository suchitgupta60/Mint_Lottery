# Setup

## Requirements

1. Ethereum Client + account with ether (testnet account OK)
1. nodeJs
1. slack team and slack bot
1. gmail account to send emails with newly created Ethereum account so slack team members

### Setup Ethereum
You need some ethereum client. We used geth (go-Ethereum) for our setup. The following geth versions were used:

* 1.6.0 (stable)
* 1.6.5 (stable)

Downloads for alls OS are on the [geth-website](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum). There is also a [launchpad projekt](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum) for deb-packages (Debian/Ubuntu) systems. You can even install the langauge compiler for solidity: solc.

Start geth for the first sync (or the rinkeby-testnet):
		
	geth --rinkeby console

Note that there is also ```--testnet``` as testnet parameter; but this parameter currently still points to the ropsten testnet. We use the rinkeby testnet for our setup. The rinkeby testnet is proof-of-authority base, so you will have to use the ether-faucet to retrieve test-ether.

Note that you need an "admin" account that holds the initial amount of ether that will be distributed to the accounts created for your users through slack-chat. You should be familiar with using Ethereum and the Ethereum client of your choice. We will define the commands for startup and RPC-API-exposure for geth. If you chose to use another client you will have to deal with the setup and startup by yourself.


### Setup Slack

At this point it is assumed that you already have a slack team (otherwise [create one](https://slack.com/create)).
You should create a [bot user for the team](https://api.slack.com/bot-users). At the end of the setup-procedure for bot users you will receive a token and a bot name (you can chose the name). You will need the token and the name to pass them to the node server at startup.

### Setup Gmail Email

In order to send emails to your team members (the ethereum account details will be sent via email) you need a gmail account. We recommend that you create a new account for that purpose. You need the gmail email address and the password to startup the node server. Note that you can also change the nodemailer implementation to use any other email account than gmail. We just use gmail for now because its amongst the most simple accounts to add using nodemailer.

Important Note: Don't forget to activate the "Allow less secure apps" setting in the [google account security setting](https://myaccount.google.com/security). If you don't activate this nodemailer will not be able to send emails through the given gmail account.


### Setup nodejs

You just need [nodejs](https://nodejs.org/en/download/) in a version >=6.x.
The setup was tested with nodejs:

* v6.9.1 on Windows
* v6.10.2 on Linux (Debian)
* v7.2.0 on Linux (Ubuntu)
* v6.11.0 on Linux (lUbuntu)

### Setup Testframeworks (optional...)

For testing solidity-contracts you can used the following Ethereum test-frameworks:

* ~~dapple (tests written in solidity)~~
* ~~mocha + chaithereum (tests written in javascript running through web3)~~
* testrpc together with truffle and mocha


## Run the program

### Startup Ethereum Client

start geth like this:
		
	geth [--rinkeby] console

#### Expose the RPC on geth client
Once the Javascript console of geth appears you have to expose the the RPC interface.

	admin.startRPC("127.0.0.1", 8545, "*", "web3,net,eth,personal")

### Deploy Contract

Use the [solidity online compiler](https://ethereum.github.io/browser-solidity) (also called remix) to get the contract ABI (interface description) and the deployment comments for geth.
* Open Lottery.sol and LotteryEventDefinitions.sol from contracts/lottery/src files.

If you want to use remix with your running geth instance (Contract: Select execution environment: web3 provider) you have to setup the web-interface yourself as described in the [repo](https://github.com/ethereum/browser-solidity).
Try to use the zip in *gh_pages* branch and serve the content via a webserver.

For the deployment of the smartcontract you should set the minAmount parameter (of contract constructor). If you want to send an initial amount of money to the newly created contract you should set the value parameter in JSON.

### Configure nodejs-app

Go to folder:

	./nodeserver/app/
	
Copy the env.example file to .env and change the parameters accordingly.

### Startup node server

Finally to start the server by running:

	npm start

### Communication through slack chat
To communicate with the bot invite it to your channel

    /invite @slackbot
    
Then you can ask the bot for some valid commands with
    
    @slackbot help

For all commands see SlackMessageProcessor class.
