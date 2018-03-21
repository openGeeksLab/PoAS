
# Proof of Network Distersity Protocol
## Motivation
   The “Proof of work” protocol is being criticized because of the energy consumption. But regardless of the energy problem, POW is still the most essential and effective method of the block chain system. With the logic of “able man earns more”, proof-of-work is the first consensus protocol that makes a total decentralized system work. All of the other protocols are following the same logic. “Proof of stake” is the most popular one of them. Because it doesn’t need any outside resources and consumes low energy by expanding the main logic from “able man earns more” to “rich man earns more”. But “POS” mode has its own flaw such as “nothing at stake” , “stake grinding attack” and so on. An inevitable problem of “POS” is caused by the logic: “rich man earns more”. Whenever the miners spend a same amount of time, the richer ones earn more, so that the rich miners tend to spend more time to work than others. In that case, the rich miners will gradually become richer and richer and this process will be ever-accelerated until there are only the richest users left in the system. By the way, some POS protocols offer “interests” to every stake holders to replace the mining process. It seems that they resolved the problem above but those protocols are vulnerable for lack of excitation.<br><br>
   The original intention of this article is to find another “ability” to replace “stake”. Considering the following fact: because of the network delay, you have to use as many distributed nodes as possible in order to pick up distributed information in the network as fast as possible. And this is a kind of “ability” that can’t be improved by enhancing the performance of a single machine or using more electric power. But in the process of the designing, I realized that the “stake” property is essential and the best choice is to use both “ability” and “stake” at the same time.<br><br>
   Compared with “POW” and “POS”, this model has the following benefits: no hashpower competition and no high-energy-consumption; no “nothing at stake” and “stake grinding” attack; better strategy of wealth distribution than pos.
(note: Because this article is discussing the situation of total decentralized systems, protocols like “PBFT”, “DPOS”, “Ripple” will not be compared with.)<br><br>

## Scene and Characters
The picture can be imagined as a scene of the honey gathering, there are 3 types of characters
1.	Flower<br>
Each node that broadcasts a transaction could be a Flower node which is mainly responsible for answering the request of Workers and voting for the main chain. The stake that a Flower node holds is seen as “Honey” in this scene.<br>

2.	Honeycomb <br>
Honeycomb nodes are miners who have their own Workers gathering “honey” for them in the network. Honeycomb nodes use the “honey” they gathered to compete for generating blocks. Any user could be a Honeycomb node.<br>

3.	Worker(bee)<br>
Workers are attached to Honeycombs in order to detect the Flowers nearby and send a “Honey-gather” request as soon as possible. A process that a Flower answers a request from a Worker is seen as a process of “Honey gathering”.<br><br>

## Consensus Process
>1.	**Before publishing a transaction, a Flower node will broadcast a signal to preannounce it. The Workers nearby send “Honey gather” requests back when they detect the signal.**
>2.	**After receiving the first request from a Worker, the Flower packs the main chain’s (about selecting the main chain, see the GHOST protocol) tail-block “b” and the address ”m” of the Honeycomb who owns the Worker into the transaction structure , and then broadcasts the transaction. A process “Honey gathering” is completed.**<br>
*(note：We need to add those two 32-byte fields “b” and ”m” to the transaction field. And a ”balance” field is needed too.)*
>3.	**This transaction is validated and packed into a new block by another miner.**
>4.	**When a Honeycomb receives a new block, he begins to validate it. At the same time, the Honeycomb checks the field “b” and “m” of each transaction. Mark the transactions with “x” when their “m” fields point at the Honeycomb himself and mark them with “y” when   their “b” fields match the current chain’s tail-block.**
>5.	**Equally divide the balance of each Flower between all of its transactions in that block.**
>6.	**Add up the balances divided at the previous step of all the transactions marked with both “x” and “y” until the Honeycomb meets the target of generating a block. The result is denoted by “X” and the max number of statisticed blocks is 100.**<br>
*(note: The “x” mark means the vote of generating blocks, but why we also need to check the “y” mark, see Chapter “Cheats and Attacks” Article 1.)*
>7.	**Add up the divided balances of the transactions marked with ”y” only in the current block and denote the result by “Y”.**<br>
*(note: The “y” mark means the vote of the main chain and there will be some adjustment about “Y”, see Chapter “Cheats and Attacks” Article 2.)*
>8.	**The Honeycomb is trying periodically to meet the target of generating a block with a mathematical operation based on constants such as timestamps and private signatures. That is denoted by: proofFunction() < target*d*X*Y, “d” means the difficulty adjustment parameter. Other blocks received during that process, competing for the main chain, should be concurrent processed. In order not to weaken the main chain, don’t stop competing for the current chain until it is 8+ lighter than the heaviest chain.**<br>
*(note: The trying frequency should be floating, see Chapter “Cheats and Attacks” Article 1.)*
>9.	**After meeting the target of generating a block, the Honeycomb packs all of the transactions received during this period into a new block and broadcasts it. All of the “xy” marked transactions(coded within 100 blocks in order to save some space), profits of all nodes and other parameters to the proof function should be packed too to be used for verification.**
*(note: Profits distribution strategy is discripted in Chapter “Profits Distribution”)*<br>

<br>

* In order to be understood better, the steps can be briefed as:<br>
   **“The Honeycombs are constantly gathering honey all over the network and the result of the gathering is saved in the blocks. The probability of a Honeycomb wins the competition of generating a block is in proportion to the honey he gathered. The Flowers vote on the main chain with their stake every time they publish a transaction and the result is saved in blocks. The probability of a block to be accepted is in proportion to the stake voted for it.”**<br>

* Shorter description:<br>
   **“Every time they publish a transaction, the stakeholders vote with their stake on the miners to generate a block and the blocks to be accepted as part of the main chain. The more stake a block or a miner get, the better chance they win the competition.”**<br>

<br>
