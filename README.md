# PoAS:A Stake-Based Blockchain Voting Consensus Protocol
([Chinese Version](/README_cn.md)) <br>
The design primarily deals with the major problems in the PoS system, providing a solution as systematically as possible.

The main purpose of the study is to make PoS protocol more suitable for the use of cryptocurrency and become the mainstream protocol in the future.

It can be summarized to the following points:

## 1. Wealth distribution 
The wealth distribution usually obeys the Pareto’s law as the following curve shows:

<div align=left><img src="/res/3.0/q_001.png" width="70%" /></div>

If everyone’s wealth grows at the same rate, the distribution will not change. As the green line shows:

<div align=left><img src="/res/3.0/q_002.png" width="70%" /></div>

In the cryptocurrency world, an overall growth of coins means inflation. In PoS systems, inflation is an incentive for the stakeholders to work. They have to earn more coins by working in order to compensate the wealth reduction caused by the inflation. To approach the ideal curve (the green line), the ability of earning should depend on the number of coins they already have, or called stakes. It means that for some of the users who don’t have much coins, they also don’t have the ability to compensate the wealth reductions because their work costs are more than the reductions. Those users have to choose not to work and then lose their wealth gradually (assume that the total value of the coin is not changed). It is the basic wealth redistribution rule of PoS currencies. As the red line shows:

<div align=left><img src="/res/3.0/q_003.png" width="70%" /></div>

As shown above, the users owning less coins than w tend not to participate in the wealth redistribution, and they will be constantly “robbed” by the richer ones. As time goes on, the poorer users will eventually disappear. The lower value w has, the more users will remain. <br>

Considering the factors influencing the value of w, there will be the following approximate equation:<br>

w\*R *(Inflation Rate)* \/(R+1) *(The wealth reduction)* = C *(Work cost)* - F *(Transaction fees acquired by working)*  ,<br>

that is : w=(C-F)\*(1+1\/R)<br>

So we have three ways to lower the value of w: increase F (Transaction fees), increase R (Inflation rate) or decrease C (Work cost).<br>

Traditional PoS protocol works as the first way or the second way, which causes serious inflation or high transaction fees.<br>

Our design works as the third way. We reduce the work cost of stakeholders from working fulltime to logging in once in between 100 hours. That reduces the work cost to about one ten-thousandth of before, and the value of w will decrease substantially. In addition, since we have sharply reduced the value of C, we don’t have to increase the inflation rate or the transaction fees to a fairly high level. It makes the wealth distribution model of PoS systems much more acceptable.<br>

## 2. Double voting and history attack
Stakeholders participate in the wealth redistribution by handing over their stakes to the miners through an accumulating process, which will be introduced later.<br>

The miner plays the role of working fulltime maintaining the network. The maintenance work includes two parts: generating blocks and voting for branches. I am going to explain the branch voting process here because it is related to the solution of the double voting problem which is one of the most well-known problems caused by “Nothing at Stake”.<br>

Briefly speaking, the double voting problem is a strategy to vote on each fork of the chain. In PoAS systems, the miners cyclically vote for the current main branch and publish to the network as transactions which will be recorded in the next block. We ensure that the total voting information of every branch are recorded in each chain with an incentive mechanism (introduced in section 3.2.2). Thus, each vote will go public before it takes effect. Then through a specific method of counting (introduced in section 3.2.1), we guarantee that each stake will be counted no more than once no matter which part of the branch are counted. Therefore, there will be no space to make multiple votes.<br>

Another major problem caused by NaS is the “history attacks”: the attacker rebuilds a new branch from a historical block trying to replace the original one. This is caused by the PoS consensus mechanism itself but we considered another aspect of it: the property of “Stake”. Unlike the other competition resources such as electricity, hashpower or storage space, stake is an inner state of the blockchains and therefore the total amount of stake is fixed. As it is introduced above, no multiple voting, we can easily conclude that a branch shouldn’t have any competitor if the stakes voted to it are more than half of the total stakes.

<div align=left><img src="/res/3.0/q_004.png" width="70%" /></div>

As it is shown above, the stakes voted to the branch are more than the rest, then the root of this branch (with the red frame) will have no chance to be replaced, or to say, it’s finalized (introduced in section 3.2.1). The finalized block that we call save point limits the history attacks to its descendants, making the attacks much more difficult. There is another factor that helps out with the history attack problem but it will not be explained here (introduced in section 3.3).<br>

## 3. Wallet applications
Wallet applications usually don’t participate in the wealth distribution because they are not involved in the process of mining. But in PoAS, the wallet applications play an important role in the process of accumulating stakes which is the first stage of mining. So we distribute a part of the working reward to the wallets’ accounts (introduced in section 4.1), besides the wallets accumulate most of the transaction fee (introduced in section 5.1) which is not negligible because of the lower inflation rate. It will encourage the developers to be more active in making better wallet applications, which may be a progress of decentralization of development. Besides, it could encourage them to create sidechains because of the clearer profit model, which will be a progress of resolving the scalability problem.<br>

## 4 Accumulating process
There are two ways of accumulating stakes: active voting and competition with network dispersity. The former one works like the delegated proof of stake: users have to choose the miners they already know about and vote their stake to them. The latter method works more objectively and needs less operations by user: miners compete with each other for votes using the ability called “Network dispersity”. Network dispersity is a concept that reflects how many dispersed units of an institution are distributed to the network, which is also the ability to grab random signals on the network. As a honeycomb works: the more workers working out there, the better chance they find a usable flower, a Miner with more online Gatherers has the better chance to find and win the vote from a ransom stakeholder. Because it is based on the network latencies which is inevitable and non-simulatable, network dispersity could be an objective resource for the miners to compete with.<br>

It doesn’t affect the security or fairness no matter which way we choose to accumulate stakes because most of the stakes are owned by the richest users who will tend to accumulate their stakes by themselves and work alone.<br>

full paper: <br>
[https://docdro.id/uanMOWk](https://docdro.id/uanMOWk)
