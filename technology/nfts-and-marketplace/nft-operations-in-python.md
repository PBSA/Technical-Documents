# NFT Operations in Python

Describing the steps to verify the NFT\_python on local or server machine below.\


**Machine configuration** : Local or server machine.\


#### ****

#### **Step 1:** Clone NFT\_python binaries on machine

#### **Project Url :**

```
https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays/-/tree/nft
```

#### **Clone binaries on Machine :**&#x20;

```
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays.git
```





#### **Step: 2** Install virtual env on machine

**Go to project :**&#x20;

```
cd python-peerplays
```

**Run:**

```
virtualenv -p python3 env
```



#### **Step: 3** Install python requirements

**Run:**

```
source env/bin/activate
```

**then,**&#x20;

```
pip3 install -r requirements.txt
```

**And then,**&#x20;

```
pip3 install -r requirements-test.txt
```

****

#### **Step: 4** Run unit tests

**Run:**

```
python3 -m unittest tests/test_nft.py
```

&#x20;

**Expected result should be as below:**

```
(env) ubuntu@ip-172-31-13-101:~/python-peerplays$ python3 -m unittest tests/test_nft.py
Not broadcasting anything!
nft_metadata_create Success!
Not broadcasting anything!
nft_metadata_update Success!
Not broadcasting anything!
nft_mint Success!
Not broadcasting anything!
nft_safe_transfer_from Success!
Not broadcasting anything!
nft_approve Success!
Not broadcasting anything!
nft_set_approval_for_all Success!
All tests successful!
.
----------------------------------------------------------------------
Ran 1 test in 3.449s

OK
(env) ubuntu@ip-172-31-13-101:~/python-peerplays$


```

Cheers, all unit tests are passed and you successfully verify the NFT\_python
