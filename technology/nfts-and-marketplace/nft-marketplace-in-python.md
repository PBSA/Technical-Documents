# NFT marketplace in Python

Describing here the steps to verify the Marketplace\_python on local or server machine.



 **Machine configuration** : Local or server machine.



####  **Step 1:** Clone Marketplace\_python binaries on machine

**Project Url :**

```text
https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays/-/tree/nft
```

#### Clone binaries on Machine:

```text
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays.git
```

\*\*\*\*

\*\*\*\*

####  **Step: 2** Install virtual env on machine

#### Go to project**:**

```text
cd python-peerplays
```

#### Run**:**

```text
virtualenv -p python3 env
```

 

#### \*\*\*\*

#### **Step: 3** Install python requirements

**Run:**

```text
source env/bin/activate
```

**And then:**

```text
pip3 install -r requirements.txt
```

**then:**

```text
pip3 install -r requirements-test.txt
```



 

**Step: 4 Run unit tests**

**Run:**

```text
python -m unittest tests/test_market_place.py
```



 **Expected result should be as below:**

```text
(env) ubuntu@ip-172-31-13-101:~/python-peerplays$ python -m unittest tests/test_market_place.py
Not broadcasting anything!
Not broadcasting anything!
create_off Success!
Not broadcasting anything!
create_bid Success!
Not broadcasting anything!
cancel_offer Success!
All tests successful!
.
----------------------------------------------------------------------
Ran 1 test in 2.270s
OK
(env) ubuntu@ip-172-31-13-101:~/python-peerplays$
```

\*\*\*\*

