# EXEBlock Knowledge Base : How to Create Smart Contract

1. Build the project \(in accordance with the guide\).
2. Start the node \(Specify witnesses , rpc port, enable-stale-production on true in configs\).
3. Connect to the node with cli\_wallet \(./cli\_wallet --chain-id &lt;id&gt;\).
4. Create a contract. Example: contract Test { uint public sum = 0;  function math\(\) public { sum += 13; }  function \(\) payable {} } create\_contract nathan BTS "60606040526000600055346000575b60918061001c6000396000f30060606040523615603d576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063addf3804146045575b60435b5b565b005b34600057604f6051565b005b600d6000600082825401925050819055505b5600a165627a7a723058200e3049dd15bab197e97b254fdc7f286668825115b2c513faba371ca09c246ad90029" 0 1 1000000 true true  nathan - account that pays for execution BTS - asset to be used for paying the fee. bytecode - smart contract bytecode \(this example has been generated with [https://remix.ethereum.org](https://remix.ethereum.org). version 0.4.8\). 0 - amount to be transferred to the contract. 1 - gasPrice. 1000000 - gasLimit.  
5. A check that the contract has been created with: get\_all\_contracts  
6. Calling a contract function: call\_contract nathan 1.16.0 "addf3804" 0 1 1000000 true true nathan - account that pays for execution 1.16.0 - id contract. addf3804 - math\(\). 0 - amount to be transferred to the contract 1 - gasPrice. 1000000 - gasLimit.  
7. Check the change of the contract's variable \(sum\): call\_contract\_no\_changing\_state 1.16.0 nathan BTS "853255cc" \(as a result we see 0d\(hex\) = 13\(dec\)\)  
8. Check contract balance: list\_id\_balances 1.16.0  
9. Credit contract with funds: call\_contract nathan 1.16.0 "00" 123 1 1000000 true true  
10. Get contract balance again \(with 123 BTS credited\): list\_id\_balances 1.16.0  

