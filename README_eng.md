# PoND(Proof of Network Dispersity) BlockChain Project
(translated by \@nuszjj from [中文版](/README.md))
## Project Introduction
This project is aimed to build a set of blockchains with a new consensus protocol: Proof of network Dispersity, which could solve some common problems of existing POW and POS protocol to make it more environment friendly and fairer. On top of that, a series of blockchains with different tasks and functions will be established, necessary rewards will be distributed to developers within the protocol layer and simplify the business model of side-chain projects, which eventually makes this project as an open scalable system, and provides a safer, fairer and more flexible platform in the blockchain, especially cryptocurrency industry.<br>
## Proof of Network Dispersity
*(Patent NO. CN2018102289423)*<br>
### Design Background
The "Proof of Work" (PoW) protocol is being criticized because of the energy consumption. But regardless of the energy problem, PoW is still the most essential and effective method of the blockchain system. With the logic of "able people should get more pay", PoW is the first consensus protocol that makes a total decentralized system work. All of the other protocols are following the same logic, like Proof of Activity, Proof of Burn, Proof of Storage, Proof of Elapsed Time, and so on. "Proof of Stake" (PoS) protocol is the most successful one of them because it doesn’t need any external resources and consumes low energy by expanding the main logic from "able people should get more pay" to "rich people should get more pay", which makes the mining competition only among the static inner states and there is no need for power consumption. But PoS has its own flaw such as "nothing at stake" which can cause double voting problem, "stake grinding attack" and so on. An inevitable problem of POS is caused by the logic: "rich people should get more pay". Whenever the miners spend the same amount of time, the richer ones earn more, so that the rich miners tend to spend more time to work than others. In that case, the rich miners will gradually become richer and richer and this process will be ever-accelerated until there are only some richest users left in the system. By the way, some PoS-like protocols offer "interests" to every stake holders to replace the mining process. It seems that they resolved the problem above but those protocols are vulnerable for lack of incentive to maintain the network.<br><br>
The purpose of this article is to find another low energy consumption proof of ability to replace PoS. Considering the following fact: Because of the network latency, the more dispersed online terminals work together, the faster they pick up randomly distributed information in the network. And this is a kind of "ability" that I called "Network Dispersity". Which can not be improved by enhancing the performance of single machines or using more electric power. Because of the feature of unduplicatable, "stake" property plays an important role as a measurement in the protocol. Users can freely choose to participate in the maintenance of network with their abilities or with their stakes. The power of mining will be balanced by a dynamic-adjustment of the reward distribution in order to maximize the fairness and safety. <br>

### Benifits
Compared with "POW" and "POS", this model has the following benefits: <br>
* **No hashpower competition and no high-energy-consumption;** [→](#anchor-02)
* **No "nothing at stake" and "stake grinding" attack;**[→](#anchor-03)
* **Provide enough incentive to maintain the network without wealth concentration problem;**[→](#anchor-04)
* **The new way of mining competition helps change the environment of App or website development and improve the user experience of them.**[→](#anchor-05)<br>

*(note: Because this article is only discussing the situation of fully decentralized systems, protocols like "PBFT", "DPOS", "Ripple" will not be involved.)*<br><br>
### Scenarios and Characters
The whole network can be imagined as kind of canvassing scenario. There are 3 types of characters according to the node's functions.<br>
1.	Voter<br>
Each node that broadcasts a transaction could be a Voter which is mainly responsible for answering the "canvass" request of the Workers and publishing transactions with its votes packed into them. The stake that a Voter holds is seen as "votes" in this scene and the number of "votes" will determine the mining competition. Any user could be a Voter.<br>

2.	Miner<br>
Miners have their own Workers canvassing "votes" for them throughout the network and they use those votes to compete for generating blocks. Any user could be a Miner.<br>

3.	Worker<br>
Workers work for the Miners to detect the Voters nearby and send a "canvass" request as soon as possible. Workers are mostly running on the network terminals by being embedded into the client Apps or web sources.<br><br>

The network scheme is shown as below:<br>
![Alt text](/res/charactors.jpg)<br>
Figure 1: nodes & network scheme illustration<br>

### Consensus Process
>1.	**Before publishing a transaction, a Voter broadcasts a signal to preannounce it. The Workers nearby send "canvass" requests back when they detect the signal.**
>2.	**After receiving the first request from a Worker, the Voter packs the main chain's (the main chain is selected as the "heaviest" branch according to the GHOST protocol) tail-block "b" and the Worker's owner(Miner)'s account "m" into the transaction structure, and then broadcasts the transaction.**<br>
*(note：We need to add those two 32-byte fields "b" and "m" to the transaction field. And a "balance" field is needed too if other place does not store stakes of the account.)*
>3.	**This transaction is validated and packed into a new block by another miner, and then the block is broadcasted on the network.**
>4.	**When a Miner receives a new block, he begins to validate it. At the same time, the Miner checks the field "b" and "m" of each transaction. Mark the transactions with "x" when their "m" fields point at the Miner himself and mark them with "y" when their "b" fields match the current chain's tail-block.**
>5.	**Equally divide the stakes of each Voter among all of its transactions in that block.**
>6.	**Add up the stakes divided at the previous step of all the transactions marked with both "x" and "y" until the Miner meets the target of generating a block. Denote the result by "X" and the max number of accumulated blocks is the voting cycle (e.g. 6000 blocks).**<br>
*(note: The "x" mark means the vote of generating blocks, but the "y" mark also need to be checked, by doing so nobody will get any reward if they select a wrong branch, which facilitate users to be more careful in the branch selection. )*
>7.	**Add up the divided stakes of the transactions marked with "y" in the current block and denote the result by "Y".**<br>
*(note: The "y" mark means the vote of the main chain and there will be some adjustment about "Y", refer to Chapter "[Frauds and attacks Article 2.](#anchor-12)" )*
>8.	**Miners are trying periodically to meet the target of generating a block with a mathematical operation based on constants such as timestamps and private signatures. That is denoted by: proofFunction() < target\*d\*X\*Y, "d" means the difficulty adjustment parameter. If there are any blocks from the competitive branches received during this process, they should be parallel processed. In order not to weaken the main chain, Miners should not stop working on the competitive branches if the weight of the main chain is less than the competitive chain's weight plus eight, and it actually follows their own benefits (possibility to become the main chain).**<br>
*(note: The trying frequency should be floating, refer to Chapter "[Frauds and attacks Article 1.](#anchor-11)" )*
>9.	**After meeting the target of generating a block, a Miner packs all of the transactions received during the period above into a new block and broadcasts it. All of the referred transactions (coded within 6000 blocks in order to save some space), profits of all nodes and the other parameters should also be packed for verification. Mining rewards will be distributed between the Miner and all the participant Voters.**<br>
*(note: Profits distribution strategy is discripted in Chapter "[Reward distribution](#reward-distribution)")*<br>
*(note2: In terms of the extra storage space for validation parameters and reward distribution, it could be 4 more bytes in the ID and 4 more bytes in the profit of each transaction at maximum, which is fully acceptable; additional work of block validation is going through the last 6000 blocks, and it will not cause a bottleneck with proper algorithm.)*<br>

<br>

* For better understanding, the steps can be briefed as:<br>
**"Every time they publish a transaction, the stakeholders vote with their stakes on the miners and on the branches. The more stake a block or a miner get, the higher chance they win the competition."**<br>
*(note1: To control the voting frequency, it is necessary to count the block gap when the last transaction arrives at current block in each account, and take a certain granularity, such as 10 blocks, as the adjustment coefficient. From zero, every unit time, the coefficient increases proportionally, and only after a voting cycle, i.e.: 6000 blocks, around 100 hours, coefficient can be restored to the maximum; also, the same adjustment coefficient will be added to each UTXO, and transaction frequency can adjust the number of stakes with the same approach and parameters as the former.)*<br>
*(note2: The introduction of voting time granularity is to restrict the voting frequency, reduce relevant transactions when generating blocks, and increase validation rate of blocks and transactions.)*<br>
*(note 3: To improve efficiency, we could add transactions with pure voting, and users are able to decide whether they want to participate in the voting for the current transaction within the voting cycle.)*<br>

<br>
The following diagram shows the mining competition based on the network latency:<br>

![Alt text](/res/competition.jpg)<br>
Figure 2: Process of competition for the Voters<br>
As shown in the scheme above, the more dispersed online user an App has, the higher possibility it has of winning the votes(stakes). Which will help the miner win the mining competition later.<br><br>

The votes will be recorded in blocks. The following diagrams show the process of competing for the block generation (x-vote) and the main chain (y-vote) respectively:<br>
![Alt text](/res/vote_on_x.jpg)<br>
Figure 3: The process of x-votes taking effect<br>
As shown in the scheme above, the system will count votes in the existing blocks and the Miner with maximum votes has the highest chance to win the competition.<br><br>

![Alt text](/res/vote_on_y.jpg)<br>
Figure 4: The process of y-votes taking effect<br>
As shown in the scheme above, when there comes a fork, Voters will vote between the branches and the votes will be recorded in the next block. Which determines the number of votes (stakes) for both branches in the next block generation, i.e. the amount of block generation difficulty parameter Y is determined. Thus, the more votes one branch can get, the faster it will be to generate the next block, and the sooner the next block is broadcasted, the more votes a branch will get in the next round. This process will increase the speed gap between these two branches and determine the main chain in short time.<br>

<a id="anchor-01">The reason why separating the voting for the mainchain and the block generation is because it could make "stakes" decide the branch individually. Because miners will only compete for the votes of generating blocks, and votes for mainchain has none effect on their incomes. That makes it not necessary to influence objective of the voting. Besides, it is difficult to do that because the Voters only need to vote their desired mainchain according to the blockchain data. So even if the users are quite centralized, it also can protect the attacks against the main chain effectively.</a><br>

<a id="anchor-02">According to the consensus process above, if the Miners would like to get more advantages in the competition, they have to earn more dispersed workers to catch up the randomly distributed signals of Voters on the network. Thus, the competition mechanism is impossible to be simulated in single computer, which avoids the competition by only infinitely increasing the performance of mining machines and ensure the mining fairness. [benifits←](#benifits)</a><br>

<a id="anchor-03">Like PoS, this protocol doesn't need any external resources. The difference is that when the votes take effect, they are already sealed in the existing blocks. That makes the "nothing at stake" and "stake grinding" problem resolved. [benifits←](#benifits)</a><br>

<a id="anchor-04">Another difference from PoS is: Because of the existence of the Miners, stakeholders could hand over their works to them by the voting process. Miners can help to complete the competition even though the users with low stakes are not so positive. Users only need to vote once within a voting cycle to make sure that their holding stakes would not miss the voting activities, which can basically avoid the unequal wealth distribution issue as discussed above. [benifits←](#benifits)</a><br>

<a id="anchor-05">Meanwhile, the structure of Miner and Worker can be embedded in the App and website development. Hopefully it helps some developers to get out of the dilemma in which they only can get income from advertisement, or makes a contribution to better user experiences of some Apps and websites. And there are a big number of developers could be the potential miners, that will reduce the threshold for starting and helps the sustainability of blockchains.[benifits←](#benifits)</a><br>

[FAQs→](#faqs)

### Compete for the Voters
To achieve the ability of faster block generation, the Miners will try the best to win as many as possible Voters. Other than using more Workers as canvassers, some of the Miners may reach agreements with some Voters to cooperate mining for their own benifits. In this case, the protocol provides some alternatives in the transaction structure, with which the Voters can choose type A: accept vote canvassing from Workers with the form of broadcast-request; or type B: designate one Miner and make him win the voting; or type C: designate themselves and work on mining individually. The regular Miners will compete with the Miners with cooperating Voters and the individual ones. Between type A and type B Miners, it is a competition of the ability of gathering users, and between type C and the other two types, it is a competition of stakes. Essentially, the three types of Miners compete with each other using the stakes gathered in three different ways. In order to balance the mining power of the three different abilities, dynamic-adjustment mechanisms are introduced through the process of reward distribution. With a dynamic balance, the risk that attackers control the network by dominating one kind of ability will be effectively reduced.<br><br>
#### Wallet application
Because of the special user group, Wallet applications can not only guide but also control the Voters to choose the transaction type and voting targets. However, considering their own benefits, wallet applications would try the best to make the Voters vote to the Miners belong to the suppliers, which voids the chances for other ordinary Miners to participate in the competition. Therefore, there needs a mechanism to encourage wallet applications to select type A to provide a fair competition environment. In this case, I added the field of "wallet account" into type A transactions, and distribute some of the profits to the suppliers of wallet applications, but type B and C don't have this kind of rewards. Since the wallet applications have a legitimate income, any behavior that influences the fairness of the competition in type A should be considered as malicious. One extra benefit if we do like this, it can encourage the developers to make better wallet apps or sidechains to provide incentives for open platform. (refer to "[Multi chain](#multi-chain)" section)<br><br>
#### PoS variant
We can also derivate a variant for this protocol: when there is only type C, every Miner mines with their own stakes and it becomes pure PoS consensus. This variant system also can ensure the fairness and solves several common problems under the traditional PoS protocol except the wealth distribution issue.<br><br>

### Reward distribution
If the distribution strategy of mining rewards is different, users may make different behaviors and that will affect the network structure. The distribution strategy is only for profits from A type mining, because profits of type B are distributed by the Miners, and there is no distribution requirement for type C mining. Considering the questions as following:

1.	To avoid that the Miners divide themselves to get more benefits, Miners should achieve rewards according to the fixed proportion of total amount.
2.	The weight of stake while distributing will influence the Voters' choice, and absolute fair distribution may not be enough to attract more ordinary Voters if we follow exactly the stake proportion. But if the stake weight is too low, we would lose too many high-stake users.
3.	After decreasing the stake weight, it is necessary to make transaction fee as reference to avoid the Voters dividing their stakes to get more rewards, and it also can provide the Voters with more strategies to choose.

Therefore, the best solution for distribution plan of type A mining is to be affected by both parameters of stake and transaction fee, e.g. the Miner shares 20% of the total rewards, and the rest will be divided into two parts, 1/3 of which is distributed according to the proportion of transaction fee, and the other 2/3 is distributed according to the proportion of stake. 25% of the rewards from Voters (i.e. 20% of total amount) will be distributed to the wallet account.

However, as for distribution plan of type B mining, it is determined by the miner according to the agreement between the Miner and the cooperating Voters. It is relatively more free, but there are also some restrictions: to ensure the benefit of the wallet applications (refer to section of "[Wallet application](#wallet-application)") with type A is always higher than that with type B, B type Miners' rewards must be restricted under the wallet income out of type A mining, in this case, under 20%.

In order to balance the power of mining, the reward proportion of type A Miners should be dynamically adjusted according to the sum of stakes of each type of transactions: decrease the proportion if the sum of stakes of type A is too high to reduce the number of A-type Miners; if otherwise increase it to attract more Miners of type A.

The default transaction fee of different voting plans could be dynamically adjusted according to the average number of Miners in type A or type B. If there are more Miners joined, reduce the transaction fee to attract more Voters to match the number of the Miners. Actually, even if there is no adjustment above, the economic principle would also help people to make such balance, just a little slower.

### Frauds and attacks
1.	<a id="anchor-11">Filtered transaction, Miners only pack the transactions which are beneficial to them.</a><br>
Because the transactions included in each block could influence the competition environment, miners may want to gain more advantages through choosing some of the transactions.<br>
First of all, transactions in each block only take up small percentage of total amount, so there will be little influence on the statistic results if some changes are made in one block. To solve this kind of problem better, we need to count the sum of stakes of all the Voters in this transaction, which will affect the next block generation interval. The higher stake it has, the shorter calculation period it will have, otherwise it is longer. E.g. if we make the calculation period between 0.9-1.1 secs, it would directly affect next block generation. In this case, it would be better choice for packing as many transactions as possible, which could disincentive the miner frauds. This parameter should be adjusted per the average value every once in a while.<br>

2.	<a id="anchor-12">It's also filtered transaction, but the purpose is to influence the branch growth speed.</a><br>
Because the transactions included in each block will also decide the speed of next block generation in the same branch by influencing the parameter "Y", and attackers could decrease the transactions deliberately in a certain branch to affect determination of the mainchain.<br>
Solution: In step 4 of the consensus process, when checking the transaction field "b", other than marking the transactions when they match the tail of the current chain as "y", we also need to mark the transaction that points to the current chain's countdown second, third (quantity adjustable) block as "y1". Both "y" and "y1" marks shall be counted when calculating the value of "Y". In this case, even though the attacker reduces the speed of a certain block generation, it will compensate the deliberate missing transactions if the latter miners are honest and all those missing transactions' "b" fields will point to the countdown second block and backwards.<br>
A doubt may come out: is it possible to count transactions marked with "x" and "y1" when calculating "X" parameter? In this case, even though somebody makes fault during packing the transactions, there will be no effect on the results because the latter miners can make compensation for that. The answer is no because if we do like this, the nodes will evade the crucial point when there are forks and vote for the blocks before that to make sure they are right. This can interfere the competition between branches, which makes it hard to determine the main chain promptly.

3.	<a id="anchor-13">51% attack</a><br>
If anyone controls over 50% stakes of the network, the system will be vulnerable under his attack, which is the same as PoS protocol. But PoS system can’t keep a high participation rate because users have to run full nodes and keep them online to join the competition. Therefore, lower proportion of online stake reduces the security level. In PoND system, stakeholders could join the network maintenance with only one vote during a voting cycle. With a lower requirement of participation, we can keep a higher online proportion of stake and that improves the security of the system.

4.	<a id="anchor-14">Simulate Workers, trying to create large number of Worker nodes by simulation and add those virtual nodes to the p2p network to increase the probability of successful canvass.</a>
To deal with this situation, we can make some control when creating the p2p links, e.g. each node only needs to build connection with a certain number of nodes with the fastest response speed.

### Block rate
The same as Bitcoin, we are fully decentralized system, so there will be some restrictions on the block generation speed. However, under this consensus mode, the capability of block generation is actually closely related with network respond time. The Miners with higher chance to generate blocks will have shorter time to receive broadcast of new blocks, while it doesn't matter much if the Miners which have lower ability of block generation receive the broadcast with some delay. In addition, I switch the main chain selection protocol from POW to GHOST, which can be more effective against attack after producing orphan blocks. Thus, the consensus period in our system may be a little shorter than Bitcoin. (we still lack experimental data for the appropriate block rate, maybe one minute)

If we want to achieve kind of blockchain with high circulation rate in which the blocks are generated in turns among a few nodes, like DPOS, it only needs some modifications in another variant of this consensus. When there are only type B miners in the system, our consensus itself is one kind of voting mechanism and we can modify based on that, e.g. simplify validation process, cancel competition mechanism and reward distribution, cancel the voting of main chain and so on.

## General plan of the project
### Multi chain
Because of very low cost for merged mining with this consensus protocol, it only needs one more thread to verify one more chain. Also, the computing power attack will not occur since there is no parasitism. Thus, we have more advantages over POW to run side chains.

The degree of decentralization in blockchain determines how robust it is, but it also restricts the speed of network synchronization, which limits the block generation speed as well. Robust and transaction volume become two contradictory features in the blockchain development. On the other hand, the more complex design blockchain has, the more function it can implement, but at the same time, it will bring more insecurity factors due to complicated codes. It is quite difficult to build a perfect blockchain, so it could be a good idea to process data with different requirements by putting them into different chains. Users could store most of their funds in the main chain with full decentralization, more security but lower speed. Then part of their fund will be transferred to certain side chain for micro transactions or other business. Side chain could be designed as high-speed chain which scarifies some security but improves the performance (refer to section of "[Block rate](#block-rate)"), or more complicated business chain with smart contract, and so on. It could also be a good choice if pegged into other blockchains.

Meanwhile, because it distributes some rewards to developers as incentive ( refer to section of "wallet application") on the protocol level and hopes this could encourage more developer teams join the development of side chain to bring fresh blood to the healthy growth of the whole system.

### Upfront plan
So far, our plan is to get two chains, one of them is used to provide the most secured cryptocurrency protection and trading function, and the other business chain would be  implemented as decentralized exchanges (refer to "DEX" for more details). We may give priority to the development of high-throughput micro transaction chain and the business chain that can run smart contracts depending on the team situation and technical details. And optimize the interface standard to make it more convenient to interact with other projects, as mentioned earlier, the target of this project is to construct a scalable multi-functional blockchain and it is certainly essential to realize value transfer between different developers and projects.

## FAQs
1. Q: The system involves in stakes, is it a variant of PoS?<br>
A: no, even though we use stake system, the key philosophy of PoND is to take advantage of network distribution to acquire user's voting, so the competition is through the capactities to achieve votes, but not the stakes. ([other differences→](#anchor-03)) Nevertheless, the PoND consensus can have the same effects with PoS after being simplified. Refer to the section of "[PoS variant](#pos-variant)" for more details.
2. Q: If Miners collaborate with Voters, they can achieve votes without using network latency. Does it break the fairness of the competition easily?<br>
A: The cooperation can indeed escape the competition of network latency. However, it doesn't break the fairness because from the perspective of the ability to achieve votes, it is the same result with either network distribution or collaboration. Refer to the section of "[Compete for the Voters](#compete-for-the-voters)". 
3. Q: Generally speaking, people may make frauds in the online voting with simulation. How can you prevent that?<br>
A：It makes simulation meaningless when we involve stakes in because nobody can simulate the stakes otherwise the cryptocurrency will not be existed. Also, users are not allowed to gain more voting opportunities by transferring their funds, which has been accomplished in PoS protocol, so we will also follow that. In addition, we have some prevention on the node simulation, please refer to "[Frauds and attacks](#frauds-and-attacks)" for more details. 
4. Q: If somebody gathered a large number of users, will that affect the network security?<br>
A: Let's answer the question from two aspects. 1. We will try the best to ensure fair competition among the miners to avoid this centralized situation. See the section of "[Compete for the Voters](#compete-for-the-voters)" and "[Wallet application](#wallet-application)";2. We separate the voting of block generation and main china to make sure even if users are very centralized, it can also maintain the network security,refer to [→](#anchor-01). 
5. Q：Since the system uses voting mechanism, what is the difference between other voting protocol, like BFT?<br>
A: First of all, the voting logical of BFT is to reach the consensus of "Majority wins", while the voting logical of PoND follows Satoshi Nakamoto's consensus logical which is "able people should get more pay". This is the most essential difference; Secondly, the BFT voting system is among few validation nodes, which is not full distribution system, but voting of PoND can be conducted in any of the nodes, it can be seen as completed distribution system like PoW.
