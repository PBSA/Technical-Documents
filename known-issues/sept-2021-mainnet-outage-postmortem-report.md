---
title: Sept 2021 Mainnet Outage Incident - Postmortem Report
---

# Sept 2021 Mainnet Outage - Postmortem Report

## 1. Background

The Peerplays Mainnet \(Alice\) network experienced a chain halt on 10-September-2021 at 10:27am GMT. A chain halt is an error condition of the network where no new blocks are produced and therefore no transactions or operations can occur. The halt is the effect of witness node software running \(or not running\) on multiple node servers and not being able to form chain consensus. When consensus breaks down, a halt or chain fork will occur.

## 2. Incident Timeline

The timeline represented here will start prior to the halt and end at the incident resolution. The time of incident resolution is defined as when a public announcement was made declaring that mainnet was back online and provided connection details to witnesses not directly involved in the incident resolution.

The entire incident happened between 09-September-2021 10:00pm GMT and 16-September-2021 5:06pm GMT. The total incident duration was 6 days, 19 hours, and 6 minutes.

### 2.1. Key Events

1. 09-September-2021 10:00pm GMT - The GPOS sub-period elapsed. Stale votes were removed for Witnesses. A new set of Witnesses became active. The Last Irreversible Block \(LIB\) stopped incrementing at block \#43328077.
2. 10-September-2021 10:27am GMT - Blocks continued to be created until 10,000 blocks past the LIB, block \#43338077. Blocks could no longer be created and the chain halted.
3. 10-September-2021 11:21am GMT - Jbahai and others noticed that Mainnet was having issues. Sofie found that the chain had halted.
4. 10-September-2021 02:10pm GMT - Surgeon proposes a "super-node" approach.
5. 10-September-2021 08:01pm GMT - Robert.Hedler finishes setting up the super-node. Witness keys are gathered and put in the super-node config.
6. 12-September-2021 08:21pm GMT - Witnesses begin to sync with the super-node.
7. 12-September-2021 09:51pm GMT - Sofie and other Witnesses run into sync issues with the super-node.
8. 13-September-2021 02:24am GMT - Hiltos connects to the super-node cli wallet and votes in active Witnesses.
9. 13-September-2021 12:18pm GMT - Surgeon, Hiltos, sieRRa19xx, Robert.Hedler, Jemshid, and Bobinson begin to collaborate in a Rocket Chat direct message channel.
10. 13-September-2021 01:38pm GMT - Surgeon discovers rouge nodes are connecting to the network which are causing the syncing issues. A clean database up to the chain halt \(block \#43338077\) is made publicly available.
11. 15-September-2021 01:24pm GMT - A Firewall is configured to protect the super-node from rouge p2p nodes.
12. 15-September-2021 06:46pm GMT - Hiltos takes over block production for the hiltos-witness account from the super-node.
13. 15-September-2021 10:41pm GMT - Robert.Hedler takes over block production for the robert-hedler account from the super-node.
14. 16-September-2021 01:03pm GMT - The Firewall is removed from the super-node.
15. 16-September-2021 05:06pm GMT - A public announcement was made in the witness-info channel in Rocket Chat that Mainnet was back online. Seed nodes and checkpoints were provided to configure the witness node program.

### 2.2. Key Actions Taken by Responders

Discussions, both public and private, happened throughout the incident on Rocket Chat channels and Telegram channels.

**Surgeon**: Offered the super-node approach to solve the incident.

**sieRRa19xx**: Collaborated with Surgeon and Hiltos to solve the incident.

**Hiltos**: Collaborated with Surgeon and sieRRa19xx, and voted active Witnesses into place.

**Bobinson**: Coordinated with Witnesses in Rocket Chat and Telegram. Helped troubleshoot syncing issues.

**Robert.Hedler**: Set up the super-node machine / environment. Collaborated to set up the super-node firewall.

**Jemshid**: Collaborated with Witnesses to continue block production.

**Sofie**: Identified the incident and collaborated on the solution.

**Jbahai**: Alerted the team to the issue and relayed info with witnesses.

**Hbelakon**: Monitored the issue and helped to coordinate work tasks to give the issue top priority.

## 3. Superficial & Root Causes

### 3.1. \(Root\) Low Number of Engaged Community Members \(Low GPOS Voting Turnout\)

One main factor of this incident was that only one community member voted within the six months leading up to the incident. Because of the diminishing nature of GPOS voting, after six months of not voting, the vote power provided by a voter is completely removed. This means the one voter within the six months was the only vote power left. This drastically shifted the active witnesses to include many non-producing \(and otherwise inactive\) witness accounts.

A larger number of engaged community members, and therefore a larger GPOS voting turnout, would have prevented this incident. Recent votes will continue to maintain the full strength of their voting power. This would keep a sudden shift of active witnesses from occurring.

**Recommendations**:

* Change the consensus algorithm to the new Proof-of-Pulse model.
* Develop awareness in the community through outreach.
* Introduce notification features in upcoming dapps \(Peerplays DEX\).

### 3.2. \(Superficial\) Information About Witnesses is Hard to Find

The ability for community members to vote with confidence depends on the quality and availability of information about the node operators. Key Performance Indicators \(KPIs\) are not standardized or tracked for node operators \(except for total missed blocks\). It's also difficult to gage the level of dedication or engagement node operators have.

Additionally it's currently not possible to tell the state of an inactive node. Some inactive witnesses may be ready to take on block production and others may have shut down their servers. Community members may accidentally vote in a witness that no longer exists.

**Recommendations**:

* Identify and track a set of KPIs for node operators and make the information public and easily accessible.
* Provide greater visibility into the status of nodes.

### 3.3. \(Root\) Low Number of Engaged Witnesses

Similar to the issue of disengaged community members, witnesses must also remain engaged and alert to upcoming events. Inactive witnesses that are on standby should be ready to take over as an active node. If previously inactive nodes get voted in, the nodes must be ready to produce blocks. If all listed witnesses were ready for block production the outage would not have occurred.

**Recommendations**:

* Node operators should be able to set their nodes to maintenance mode or similar function, like SON nodes.

### 3.4. \(Superficial\) Lack of System Fail-safes

The chain has no fallback system. This is a tricky topic because the intention is to remain decentralized and a fallback would most likely be under the control of one organization. But the risk of not having a fallback system may just outweigh the need for total decentralization. A fallback could be as simple as making all the "init" accounts the active witnesses until voting resumes. This could keep the chain moving along to prevent the financial losses people could experience during an outage.

No disaster recovery protocol existed at the time of the outage. This hindered the recovery of the network.

**Recommendations**:

* Investigate feasible fallback mechanisms to include into the chain.
* Develop a disaster recovery protocol to bring the chain back online as quickly and safely as possible.

## 4. Impacts

The known impacts of the mainnet outage are as follows:

* The Peerplays mainnet network was down for 6 days, 19 hours, and 6 minutes. No transactions could occur during this time.
* Community members could not transfer tokens or coins, vote for node operators, use any Peerplays dApp, or access their wallets.
* Exchanges that are integrated with Peerplays could not access their Peerplays wallets, deposit, or withdraw funds.
* External third-party programs and websites using the Peerplays API could not access chain data.
* Witnesses were not producing blocks and so were not given block production pay during the outage.

Although the impacts were far reaching in scope, the damage was minimal because of the low utilization of the network as evidenced by the low level of transactions over the preceding months. Brining the mainnet back online restored functionality and has mitigated the impacts listed above.

## 5. Incident Resolution

The Peerplays mainnet was back online 16-September-2021 05:06pm GMT.

The solution, step-by-step:

1. Create a super-node, a single node running all the witnesses, using a snapshot of the database from the last irreversible block \(LIB\).
2. Firewall it off from the internet.
3. Create several seed nodes on the same machine which sync from the super-node.
4. Allow the super-node and new seeds time to create blocks well beyond the LIB.
5. Allow engaged witnesses through the firewall to sync their nodes with the seed nodes. \(witnesses use the database snapshot.\)
6. Witnesses begin to take over their block production from the super-node, becoming new seed nodes as well.
7. When all active witnesses resume their block production, the super-node can be shut down.

## 6. Lessons Learned

Ultimately, the higher the risk that a mainnet outage would cause also means the system will be more resistant to such issues. A high level of network utilization means a more engaged community. Bringing more value to the network by providing dApps and onboarding more third-party solutions will do more for protecting the network than trying to fool-proof the system in its early stages.

The most good that can be done for preventing further incidents such as this will be in planning for system recovery. Until the network is highly utilized, having protocols in place to shorten the downtime would be the best use of our time.

