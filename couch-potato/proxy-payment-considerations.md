# Proxy Payment Considerations

The Couch Potato concept relies on a number users operating as data feed providers for their own data proxies \(Couch Potato instances\).

In return for the Couch Potato user's participation they would rewarded according to a payment structure.

This document proposes how this payment structure could work and the challenges of a 'definition of success'.

At it simplest the payment structure could be something like, for every successful incident a user submits they'll be paid $1. On paper that's simple, but what do we mean by a 'successful' incident.

BOS operates by receiving a series of incidents from all of the Data Proxies. These incidents are sometimes called triggers or messages, but basically they're just JSON sent to BOS though a RESTful web service. What's important to us are the number of incidents and what they represent, as follows:

| Incident | Description |
| :--- | :--- |
| create | Sent each time a game is created |
| in\_progress | A game is started, `whistle_start_time` sent to BOS |
| result | A game's result is added, `away_score` and `home_score` sent to BOS |
| finish | A game finishes, is settled, `whistle_end_time` sent to BOS |
| canceled | A game is canceled / postponed. |

For more information see:

{% page-ref page="api-1/bos-schema.md" %}

The definition of success for each incident is quite simple:

| Incident | Success |
| :--- | :--- |
| create | A game was created at the correct scheduled start time. For example, an NFL game is scheduled to start at 7pm EST on 12-12-2020. That is the date/time that must be entered. |
| in\_progress | The ACTUAL start time \(not scheduled time\) of the game entered within 5 minutes |
| result | The result of the game, usually entered at the same time as the game finishes. |
| finish | The finish time of the game, entered within 5 minutes |
| canceled | Not time specific since a game could be canceled before start time or while in progress. But the canceled incident should be sent as soon as possible. |

Based on this criteria it _could_ be possible to pay a user per successful incident submitted, but we shouldn't because:

* If any of the first four incidents aren't sent to BOS, or incorrectly sent, then the game might not settle correctly \(depending on what the other data proxies submit\).
* User's shouldn't be rewarded because of the success of other data proxies.

**Proposal** - User's should ONLY be paid for correctly submitting all four incidents.

For this system to work we must be able to find data that corroborates that all of the incidents were correctly sent, this involves knowing:

1. Each incident was included in the consensus by BOS when all data proxy incidents are compared.
2. In total, all incidents passed consensus and these incidents would result in a successfully settled game.

It should be possible to use the data stored in a BOS database to identify incidents by proxy, but we do need to overcome some challenges:

1. It doesn't require ALL data proxies to submit correct incidents for consensus to be reached by BOS. For example, if only two data proxies are needed to submit the same start time for a game, BOS would approve it. What we don't want to do is penalize any data proxy that sent a correct start time, or any other incident, just because their incident wasn't needed for consensus.
2. The same could apply for the complete settlement of a game. The user could correctly submit all four incidents, but BOS consensus could be reached without the need for all incidents from the user. Again, we shouldn't penalize a user who sent a perfect set of incidents.

As long as these challenges can be overcome, and we have accurate log of a user's incidents, then we should have our 'definition of success'.

We also need a payment strategy for canceled games. Unlike successfully finished games, to cancel a game requires only the one incident to be sent. We shouldn't be rewarding any DP user for not canceling a game, but a successful cancelation is a much simpler process than sending four,  time sensitive, incidents for a settled game.

**Proposal** - User's should be paid a smaller amount \(say 50%\) for submitting a successful cancelation.

### Gamified Payments

A very exciting option put forward is to recruit, and pay, data proxy users in the same way as Witnesses, Advisors and Proxy voters.

A Data Proxy operator would be voted for in the same way as the other Peerplays operators. And could then be rewarded in the same way. Voting for a Data Proxy would also constitute a successful GPOS vote.

This system would have the following benefits.

* Another GPOS voting option for token holder.
* Much better control over possible collusion amongst Data Proxies as they would have to be voted in rather then PBSA, or Witnesses, having to decide on trusted proxies.
* Bad proxies would be voted out, again, through a democratic voting.
* Payment for Data Proxies could be in PPY, the same way that Witnesses are paid but on a different pay scale.

While this system wouldn't be available at first release, it is definitely worth serious consideration for later releases.

