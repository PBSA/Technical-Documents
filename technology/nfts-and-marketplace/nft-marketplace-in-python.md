# NFT marketplace in Python

Describing here the steps to verify the Marketplace\_python on local or server machine.



&#x20;**Machine configuration** : Local or server machine.



#### &#x20;**Step 1:** Clone Marketplace\_python binaries on machine

**Project Url :**

```
https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays/-/tree/nft
```

#### Clone binaries on Machine:

```
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays.git
```

****

****

#### &#x20;**Step: 2** Install virtual env on machine

#### Go to project**:**

```
cd python-peerplays
```

#### Run**:**

```
virtualenv -p python3 env
```

&#x20;

#### ****

#### **Step: 3** Install python requirements

**Run:**

```
source env/bin/activate
```

**And then:**

```
pip3 install -r requirements.txt
```

**then:**

```
pip3 install -r requirements-test.txt
```



&#x20;

**Step: 4 Run unit tests**

**Run:**

```
python -m unittest tests/test_market_place.py
```



&#x20;**Expected result should be as below:**

```
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

****
