# EXEBlock Knowledge Base : Curl Commands form Kiegan.

Kiegan's only comment was to change "[https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws)" to point to the blockchain we are using.

The following cURL call on the Dan Notestein testnet return results.

curl --data '{"method": "call", "params": \["database", "get\_unmatched\_bets\_for\_bettor", \["1.21.263", "1.2.51"\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "list\_sports", \[\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "list\_event\_groups", \["1.16.0"\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "list\_events\_in\_group", \["1.17.0"\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "list\_betting\_market\_groups", \["1.18.48"\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "list\_betting\_markets", \["1.20.26"\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["bookie", "get\_binned\_order\_book", \["1.21.263", 4\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["bookie", "get\_binned\_order\_book", \["1.21.259", 4\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

curl --data '{"method": "call", "params": \["database", "get\_objects", \[\["1.11.809"\]\]\], "jsonrpc": "2.0", "id": 1}' [https://peerplays-dev.blocktrades.info/ws](https://peerplays-dev.blocktrades.info/ws) \| json\_pp

**To get a user account**

This will return the account object for 'jim-gill'.

```text
curl --data '{"method": "call", "params": ["database", "get_account_by_name", ["jim-gill"]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

**To get a list of accounts**

This will return a list of the first 100 accounts.

```text
curl --data '{"method": "call", "params": ["database", "lookup_accounts", ["", 100]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

**To retrieve matched bets**

This will return the matched bets for account 1.2.51.

```text
curl --data '{"method": "call", "params": ["bookie", "get_matched_bets_for_bettor", ["1.2.51"]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

**To retrieve bet objects**

This will return bet object 1.22.694 and 1.22.695.

```text
curl --data '{"method": "call", "params": ["bookie", "get_objects", [["1.22.694", "1.22.695"]]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp 
```

**To retrieve a list of assets**

This will return a list of assets on the blockchain. _I'm not sure of the purpose of the \("A",8\) parameters_.

```text
curl --data '{"method": "call", "params": ["database", "list_assets", ["A",8]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

  
**To Get the fee schedule for a particular Blockchain**  
This will return a list of fees for the blockchain.

```text
curl --data '{"method": "call", "params": ["database", "get_objects", [["2.0.0"]]], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

  
**To Get the global betting statistics**  
This will return a small JSON object containing the global statistics of all betting objects on the Blockchain

```text
curl --data '{"method": "call", "params": ["database", "get_global_betting_statistics", []], "jsonrpc": "2.0", "id": 1}' https://peerplays-dev.blocktrades.info/ws | json_pp
```

