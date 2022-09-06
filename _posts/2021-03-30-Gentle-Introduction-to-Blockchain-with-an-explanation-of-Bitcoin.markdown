---
layout: post
comments: true
author: islimani
title: A gentle introduction to Blockchain with a simplified explanation of Bitcoin
date: 2021-03-30 18:00:20 +0300
words: 1610
category:
        - Blockchain
summary: With the recent rise of cryptocurrencies in value, Bitcoin reached more than 19000$ by late December 2017, much fuel was added to the hype surrounding the technology behind this success. After all, transferring billions of dollars securely and without a central authority is a revolution per se.
thumbnail: /assets/img/step1.png
---


# INTRODUCTION

With the recent rise of cryptocurrencies in value, Bitcoin reached more than 19000$ by late December 2017, much fuel was added to the hype surrounding the technology behind this success. After all, transferring billions of dollars securely and without a central authority is a revolution per se.

If transferring funds securely in real-time with minimal charges, exchanging other assets is a piece of cake. This is why a debate is happening about the potential impact of the technology on different industries: financial, Supply chain, insurance, energy management and real estate. Some see it as a bless and others like a curse. And this is exactly the same bad mentality that let old style businesses went to history while others emerged. Technology is a tool, it’s a bless or a bless. While Internet revolutionized the world in the 90s, it didn’t replace old business entirely. It just improved what’s already existed. This is why, if x would do you better, then this is good for your business, brace yourself and start your PoC (proof of concept). Otherwise, you have nothing to worry about.

But despite our opinions, IBM backed by more than 185 big companies will not sit idle by. It has introduced it’s open source framework for developing enterprise blockchain applications: HyperLedger Fabric and Composer. A framework where Bitcoin “could be a simple application built on the fabric”. [^1]

[^1]: https://openblockchain.readthedocs.io/en/latest/protocol-spec/?q=bitcoin&check_keywords=yes&area=default#4.-Security

Banks also start playing around with the technology including J.P Morgan Chase [^2], Citigroup, HSBC and Deutsche Bank [^3].

[^2]: https://www.techworld.com/picture-gallery/business/bitcoin-beyond-how-banks-are-investing-in-blockchain-technology-3625324/
[^3]: https://www.db.com/newsroom_news/2017/deutsche-bank-partners-with-ibm-for-block-chain-based-shared-kyc-platform-en-11726.htm

# The RISE of BLOCKCHAIN

The idea of having a decentralized system that excludes any middlemen and central agencies has been around for decades. This could have ample advantages: Ensuring trust between parties, increased transparency, reduce middlemen costs  and improve speed.

David Chaum proposed between the 80s and 90s an electronic-cash with high degree of privacy but it failed due their **reliance on a centralized intermediary**.

Wei Dai proposed an e-cash in 1998 through **solving computational puzzles as well as decentralized consensus** but lacked details on could be implemented.

Hal Finney in 2005 lays the foundation for the **Proof-of-Work algorithm** used in bitcoin but the solution **relies on trusted computing** as a backend.

The problem with all the existing solutions are either they are not fully decentralized or they lack a proper Proof of concept, until 2009 when Satoshi Nakamoto proposed Bitcoin as the first fully decentralized currency.

Satoshi didn’t solely propose a solution but he also implemented it. Now with its 9 year anniversary, we could judge if the technology underlying the solution is mature enough to incorporate it into other existing systems.

> **Note**: For more infos check [here](https://github.com/ethereum/wiki/wiki/White-Paper)

# KEY CONCEPTS
## What's a blockchain

The blockchain has four definitions, depending on the context and who is defined it:

1. For the wide 99% of the public, Blockchain is associated with Bitcoin. So a Blockchain is the Bitcoin. And because Bitcoin is not regulated, Bitcoin isn’t legal, so the blockchain!
2. In sample English terms, Blockchain is an immutable huge file, with hundreds of GBs[^4],  that records all the transactions that have occurred on the network.
4. Generally speaking, a blockchain is all the technologies, standards and tools used to fully operate a fully decentralized network to exchange transactions securely.
5. Technically speaking, a blockchain is a continuously growing list of records, called blocks, which are linked and secured using cryptography. Each block typically contains a cryptographic hash of the previous block, a timestamp and transaction data. By design, a blockchain is inherently resistant to modification of the data. It is **an open, distributed ledger that can record transactions between two parties efficiently and in a verifiable and permanent way**.[^5]

With the advent of Ethereum platform to create decentralized applications, thanks to Vitalik Buterin, Blockchain 2.0 emerged. This is one way to say, just focus on your business needs using one of the supporting scripting languages, we take care of all the hassle. It's just silly  to build the network from scratch everytime we want to build a blockchain based solution.
> **ProTip:** You can also build your application on top of Bitcoin, but it has limitations.[^6] [^7]

## Public Key Cryptography

A cryptographic system that uses pairs of keys: public key exposed to everyone and private key kept which is only known to the owner. TLS or SSL use a public key cryptography to exchange a secret key to encrypt data between two parties.

## Hash Function

Have you ever heard of md5 or sha1 then you are good to go. If not a hash function takes an input of arbitrary size of data and map it to data of fixed size.

	Sha1(ilias) = efc99421dcd5b83eecc5de58b1c417be4e57d4c3

# HOW DOES  BITCOIN WORK?

## FIRST STEP: USER CREATES TRANSACTION

![Figure 1 User Creates transaction]({{site.baseurl}}/assets/img/step1.png)

A transaction in Bitcoin is what tells the network that someone who has funds want to transfer them to another one. A simplified transaction looks like this:

![Figure 2 higher level abstraction of transaction]({{site.baseurl}}/assets/img/transaction.png)

After constructing the transaction, it is signed with the owner private key as a proof of ownership to deter anybody from spending his funds. Nonetheless, to verify the integrity of the transaction if it doesn’t tampered with, Bitcoin add the hash of the current transaction.

Also if you notice, the public key will play the role of an address of both sender and receiver.
### Transaction inputs and outputs

Transaction inputs and outputs are the basic block behind transactions in bitcoin. There isn’t a concept of balance in Bitcoin. In fact, the real transaction doesn’t look at all like what we saw before, but that’s just another story.

So how does the network keep track of your balance? Here Satoshi introduced the concept of **UTXO** or **unspent transaction outputs**. Every time you send money a transaction output is created. Actually outputs. Say you have 1btc and you want to send 0.5 only, Bitcoin will generate two outputs, 0.5 will send back to same address and the other 0.5 is sent to your recipient. This simply means that in Bitcoin there isn’t a concept of addition or subtraction. There are references only: you have 1 btc then at some point there is a transaction output created that shows 1btc was sent to you.

So your balance is simply all your UTXO scattered around the transactions.

![Figure 3 output transaction]({{site.baseurl}}/assets/img/output.png)

Inputs on the other hand are references to outputs from previous transaction.

## SECOND STEP: TRANSACTION IS BROADCASTED TO ALL NODES IN THE NETWORK FOR VERIFICATION
Once a node receives a transaction, it starts by verifying it based on a list of criteria, e.g. the transaction size is less than the block size. This ensures only valid transactions are circulated in the network.

Valid transactions are assembled in a transaction pool waiting to get included in a block.

## THIRD STEP:  NODE STARTS COLLECTING VALID TRANSACTIONS INTO A BLOCK AND CONSTRUCTING THE BLOCK HEADER

![Figure 4 abstraction of a block]({{site.baseurl}}/assets/img/block.png)

While constructing the block header the node should add:
1. The previous Block hash
2. **Timestamp**: creation time of this block
3. **Nonce**: arbitrary integer number used to verify the block initiliazed to zero.
4. **Difficulty**: Defines the required work to make a valid block. This is adjusted depending on computational power and other factors. Since the speed of generation of blocks should remain constant at 10m on average.
5. **Merkle root**: a hash of the root of the merkle tree.

> **Note:** Merke tree is a hash tree where a node is the hash of its child nodes.

![Figure 5 Merkle tree]({{site.baseurl}}/assets/img/Hash_Tree.png)

[source](https://en.wikipedia.org/wiki/Merkle_tree)

Using a merkle tree helps in two ways: reclaim disk space and verify payments that took place. For the first Objective, Satoshi says in his white paper that “Once the latest transaction in a coin is buried under enough blocks, the spent transactions before it can be discarded to save disk space. **To facilitate this without breaking the block's hash**, transactions are hashed in a Merkle Tree ».

Once the block header is constructed, valid transactions are added to the block and the fourth step begins.

## FOURTH STEP: EACH NODE STARTS FINDING A PROOF-OF-WORK FOR ITS NODE 

Finding the Proof-of-Work for a block is called mining. 

## What's mining?

Mining simply means hashing the block by incrementing a nonce until a specific target is found, that is, finding a solution to the proof-of-work algorithm. This is Bitcoin way to achieve consensus between nodes.

This is an example of a simplified version of a proof-of-work algorithm.

```java 
    //Increases nonce value until hash target is reached.
	public void mineBlock(int difficulty) {
		merkleRoot = StringUtil.getMerkleRoot(transactions);
		String target = StringUtil.getDificultyString(difficulty); //Create a string with difficulty * "0" 
		while(!hash.substring( 0, difficulty).equals(target)) {
						
			nonce ++;
			hash = calculateHash();
		}
		System.out.println("Block Mined!!! : " + hash);
	}
```
[source](https://medium.com/programmers-blockchain/creating-your-first-blockchain-with-java-part-2-transactions-2cdac335e0ce)

In this example, the target is a string with 3 leading zeroes. The application finds the hash in 28ms. If we increase the difficulty to 5. The Time taken is 3901ms!
You can now imagine why it is difficult to cheat in the network.

## FIFTH STEP: BROADCAST THE BLOCK WITH THE SOLUTION TO THE NETWORK

Once a node finds a solution, it transmits it to all its peers. Like with transactions, nodes verify the block against a long list of criteria, e.g. block size, transactions are valid, etc. This deters dishonest nodes from cheating.

## SIXTH STEP: ADD THE BLOCK TO THE EXISTING BLOCKCHAIN

# SUMMARY
Even though the blockchain technology has a promising future, it's still in its infancy. Much work has to be done. Which is what different communities try to do thanks to everyone. 
In the next article, we will explore HyperLedger Fabric or Composer.

[^4]: https://en.wikipedia.org/wiki/Blockchain
[^5]: https://en.wikipedia.org/wiki/Blockchain
[^6]: https://github.com/ethereum/wiki/wiki/White-Paper
[^7]: https://bitcoin.stackexchange.com/questions/19287/how-to-fork-bitcoin-and-build-own-cryptocurrency
