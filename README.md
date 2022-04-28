# Maximal Extractable Value (MEV)


It refers to the maximum value that can be extracted from the block production apart from the standard block rewards.

## What is MEV?

The rewards come because Miners when creating a block can include, exclude and change the transactions of the block as they wish, this means they can favour some transactions as compared to others and gain some additional profits by doing so. Note that we are talking about Miners right now but things will change after [the merge](https://ethereum.org/en/upgrades/merge/).

## Searchers
"Searchers" are participants which are not participating in the network but are looking for opportunities to make profitable transactions. Miners get benefited from these "Searchers" because "Searchers" usually have to pay very high gas fees to actually be able to make a profitable transaction as the competition is very high. One example that we studeied was DEX Arbitrage in our flash loan example.

"Searchers" use a concept of "Gas Golfing" to be able to program transactions in such a way that they use the least amount of gas. This is because of the formulae `gas fees = gas price * gas used`. So if you decrease your `gas used`, your `gas fees` also reduces.

We talked about how to decrease gas usuage in detail in one of our previous levels.

# Frontrunners

Now the intresting part comes in where we get to talk about `frontrunners`. Frontrunners are bots which are actively looking through the mempool(transactions which have been sent by the uuser but yet have not been mined) to search for these profitable transactions that the "Searchers" were trying to do. They first test it locally to see if its actually profitable and if that is true they submit a modified transaction with a replaced address and a higher gas price than the original transaction.

# Flashbots

To prevent frontrunning and high gas prices on the network, "Searchers" can use Flashbots which is an extension of go-ethereum which allows "Searchers" to submit MEV transactions directly to the miners so that they dont have to go through the public mempool of Ethereum. It helps in preventing gas wars and also helps in keeping the network less congested by diverting the transactions to a private communication channel between the ethereum users and the miners. 

We will talk about flashbots in more detail further down


## Use cases of MEV

# Arbitrage

We have already talked about this in our previous tutorials.

"Searchers" use MEV through flashbots or by paying high gas fees to do a successful arbitrage

# Liquidations

How some defi protocols work is that you can borrow some assets but you have to over collatteralize them using some other asset.

There might be a case because of market fluctuations that the value of your borrowed assets starts exceeding a percentage(say 35%) of the value of the collateral the user supplied. Every protocol has a different percentage but after that percentage, most of the protocols allow anyone from the outside world to liquidate the borrower. Consider this very similar to how if someone doesnt pay loan on time, their house which they kept as a collateral is auctioned by the bank where the bank takes away the original value of the loan + intrest and returns back the money which is left over. Similarily when the borrower gets liquidated, some part of their collateral goes to the lender which includes intrest and the borrowed money. Along with that the borrower also has to pay a liquidation fee which goes to the user/bot which started the liquidation transaction.

"Searchers" use MEV through flashbots or by paying high gas fees to do liquidation transactions

# Sandwich Training

In this "Searcher" will watch the mempool for any transactions which will lead to a big trade and may disrupt a DEX's asset pair. Before the big trade "Searchers" place a buy order to buy the asset whose price will significantly increase after the big trade "Searchers" sell the asset at very high profit.

"Searchers" use MEV through flashbots or by paying high gas fees to do Sandwich Training

# The good and the bad

# Pros

Its good for many Defi projects because arbitrage corrects the price between multiple DEX's and protocols rely on liquidations to ensure that the borrowers dont fall below their collateral.

# Cons

The Cons is that frontrunners and gas wars result in very high gas prices for the users and its not good for the user experience. 

## TALK ABOUT BLOCK REMINING


## Solution?

We briefly talked about Flashbots above but essentially Flashbots are being built so that they can solve the Cons related to MEV. They are trying to build an ecosystem which is permisionless, transparent and fair for having better MEV and protection against front running.

The issue with the current Ethereum ecosystem is that the transaction are sent in an open mempool which is viewable by all which leads to potential for frontrunning. It also has the added disadvatage that if you send the transaction but your transaction actually doesnt get executed, you still end up losing some gas.

Instead Flashbots created something which is really cool, they decided to allow users to privately commmunicate the transaction order they want and bid the amount they are willing to give. If their bid fails, the user doesnt end up paying anything. This mechanism elimintes front running.

# Architecture of Flash bots

Lets understand further down how everything works underneath in flashbots

![](https://i.imgur.com/l5ageJ7.png)

(Referenced from the [docs of flashbots](https://docs.flashbots.net/flashbots-auction/overview))

As you can see in the diagram there are three parties involved the searcher, relay and the miner.

Essentially "Searcher" wants to send a private request to the miner so that his transactions are not viewable in the open mempool of Ethereum and thus preventing him  from front running.

The "Searcher" expresses their bids for inclusion through ethereum transactions in the form of gas price of direct payment to the coinbase address(address of the miner). Using direct payments instead of gas price allows users to make payments only if their bids are successfull. 


Now the issue comes because "Searcher" doesnt have to pay anything for the failed bids, they might do a DOS attack and spam the miners with invalid transaction bundles. To prevent this the user's transactions are first sent to the relayer who validates the bundle and also doesn bundle merging.


Note official documentation of flashbots suggests not to integrate with relayers other than the ones from Flashbots until the system is fully decentralized because relayers have access to the full transaction data and may use it for their own benefits.


The miners which want to run the flashbots runs the mev-geth code which is an extenson of the geth code. 
The miner can evaluate transaction bunndles and combine the ones which dont conflict to create the most profitable block.

Note official documentation of flashbots suggests not to integrate with miners directly until the system is fully decentralized because miners have access to the full transaction data and may use it for their own benefits.

Right now most of the flashbots work is still in research and early phases

But as we all can understand, it has a lot of potential

## Talk about eth_sendBundle

# talk about the explorer

# add some examples of the ways flashbots were used

# References

- [Ethereum.org docs] (https://ethereum.org/en/developers/docs/mev/)
- [Flashbots docs](https://docs.flashbots.net/flashbots-auction/overview)
