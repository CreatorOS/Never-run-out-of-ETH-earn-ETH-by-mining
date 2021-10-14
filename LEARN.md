# Never run out of ETH, earn ETH by mining
If you’ve been building on Solidity or been following quests on Questbook to deploy smart contracts, you’d have realized that you need ETH to do anything from deploying to interacting with the functions on the smart contract. 

One of the regular problems is that you’d usually run out of ETH. You usually have to go over to the faucets to get test ETH and many times, those faucets are down.

So, what do we do? We build our own faucet - ofcourse.
## Setting up Geth
First up, you need a computer that can run for a long period of time (typically a week) without shutting down. I recommend using a cloud machine.

You can visit [https://geth.ethereum.org/docs/install-and-build/installing-geth](https://geth.ethereum.org/docs/install-and-build/installing-geth) to install on your machine. 

Geth is now installed, let’s run it. 

But before running we have one more step.

```

geth account new

```

This will create a new account. When you earn ETH, which account will it be added to? This command will create a new account where money can start getting credited.

The private key will also get stored in `~/.ethereum`. 

Your address will be displayed on the console. Copy that and keep it handy

This public key will be your account address and the private key will be used to sign off transactions when you want to send ETH from this account to other accounts.
## Start earning by mining
Now that Geth is all set up we can start earning by mining. 

You can start mining by 

```

geth --ropsten --mine --miner.etherbase=\[PASTE THE ADDRESS HERE\]

```

Make sure you run this command on a `screen` so that you don’t accidentally terminate the process. Keep this computer on for the next few days/weeks.

This will start mining on the ropsten test network and money will get credited to the address mentioned as etherbase.

Mining is a process in which you provide your computing resources to verify the transactions on the ethereum and the network pays you back in ETH for those services.

The mining process is likely to take a couple of days.

![](https://qb-content-staging.s3.ap-south-1.amazonaws.com/public/fb231f7d-06af-4aff-bca3-fd51cb633f77/26ec0e00-83eb-47dd-b099-4b3eefc3103e.jpg)

The number represents the block number that has been processed so far. You need to wait till the age comes down to 0.

What geth is doing is it is replicating all the transactions on Ethereum Ropsten ever. All the transactions will be first replicated and only then will your geth be able to verify transactions. 

To verify a transaction, geth primarily checks whether the sender has enough money to send to the recipient. The way to figure out all the account balances is to look at all the transactions and replay them from the beginning. That is why it will take a while to `sync` all the transactions.

All the transactions are verified by being included in a block. A set of transactions is called a block. Every 15 seconds, a new block is created. The series of blocks is called the blockchain. 

At the end of the sync, you’ll have the full Ethereum blockchain replicated on your computer. 

You can check if the syncining is done by running the following command on a new terminal

```

$ geth --ropsten attach 

> geth.syncing 

```

![](https://qb-content-staging.s3.ap-south-1.amazonaws.com/public/fb231f7d-06af-4aff-bca3-fd51cb633f77/98b35b86-33a9-4a04-8c70-8c5f4bb6cc8e.jpg)

If current block = highest block means that the blockchain size is the same as the replica that has been downloaded so far. That’s when syncing ends.
## Earning
You can check your balance using

```

$ geth --ropsten attach

> web3.fromWei(eth.getBalance(eth.coinbase), "ether")

```

This would likely be zero at this point if the syncing is still in progress. 

`eth.coinbase` is the address of this geth client that you had created earlier. It is a very interesting concept - we’ll understand that by seeing how transactions are actually verified. 

There are thousands of computers who are running geth all around the world, if not millions. 

Every 15 seconds, one of these computers is randomly chosen to `propose a block`. 

Proposing a block consists of the following steps

1. Check all the transactions that have been requested but not have been approved or rejected. This set of pending transactions is called the mempool.
2. Pick a few transactions and attest whether they are valid or not. This is decided by whether the sender has as much money to send as mentioned in the transaction.
3. Package them in a block. A block is about 100KB in size. So, the computer (aka node) is allowed to put in as many transactions as it wants as long as it is less than 100kb in size. 

When the block is proposed, the rest of the computers vote on whether all the transactions have been correctly approved or rejected. If 51% of the computers say the transactions included in the block are all valid, the block is confirmed and added to the blockchain. 

There are 2 ways a computer earns ETH.

The first is, while proposing the block, the node is allowed to add one extra transaction to the block called the coinbase transaction of upto 5ETH in value. Unlike other transactions, the sender address of the transaction will be a null. 5 ETH is created from thin air. The receiver of this coinbase is the address of the geth client that proposed this block. 

So, when the block gets approved, all the computers update the balance of all the addresses on Ethereum. And thus the node proposing the block also known as the miner has 5ETH added to their balance.
## Transaction fees
There is a second way for a geth client to earn money other than mining using a coinbase transaction. 

Every transaction you send on ethereum has a transaction fees. All the transaction fees of transactions in a block goes to the miner of that block. 

This transaction fees is represented as `gas fees`.

Here’s a screenshot of a transaction on meta mask.

![](https://qb-content-staging.s3.ap-south-1.amazonaws.com/public/fb231f7d-06af-4aff-bca3-fd51cb633f77/2175f311-9928-420c-b726-f845d3cded97.jpg)

This is how the nodes are compensated for keeping their computers on through the nights and days.
## What next
In a couple of days, the full sync will be completed and you will start mining. You can check your balance using `geth attach` on a regular basis. 

Once you start earning ETH on this account, transfer some money to your metamask wallet using `geth`. 

```

$ geth --ropsten attach

> web3.eth.sendTransaction({

    from: "0xE618A4B5A516f371Ce26d9A1DBE7839F4e3812GB",

    to: "0xE618A4B5A516f371Ce26d9A1DBE7839F4e3812CB",

    value: web3.toWei(1, "ether")

})

```

Create a Faucet like this one [https://faucet.ropsten.be/](https://faucet.ropsten.be/) so that you can refill your metamask wallet whenever you want, and so can others in the community :)