# Wallet Calls

### [Wallet Calls](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id9)

#### [is\_new](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id10)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::is_new`\(\)_const_  


Checks whether the wallet has just been created and has not yet had a password set.

Calling `set_password` will transition the wallet to the locked state.**Return**

true if the wallet is new

#### [is\_locked](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id11)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::is_locked`\(\)_const_  


Checks whether the wallet is locked \(is unable to use its private keys\).

This state can be changed by calling [`lock()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a5e7950c9039f0c59e1266d6732d94e09) or [`unlock()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1ae16994a63cfdef1616b6b968117fd29d).**Return**

true if the wallet is locked

#### [lock](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id12)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::lock`\(\)  


Locks the wallet immediately.

#### [unlock](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id13)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::unlock`\(string _password_\)  


Unlocks the wallet.

The wallet remain unlocked until the `lock` is called or the program exits.

When used in command line, if typed “unlock” without a password followed, the user will be prompted to input a password without echo.

**Parameters**

* `password`: the password previously set with [`set_password()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a2a5d174ec4fde8633b8e962fabc00804)

#### [set\_password](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id14)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::set_password`\(string _password_\)  


Sets a new password on the wallet.

The wallet must be either ‘new’ or ‘unlocked’ to execute this command.

When used in command line, if typed “set\_password” without a password followed, the user will be prompted to input a password without echo.

**Parameters**

* `password`: a new password

#### [dump\_private\_keys](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id15)

map&lt;public\_key\_type, string&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::dump_private_keys`\(\)  


Dumps all private keys owned by the wallet.

The keys are printed in WIF format. You can import these keys into another wallet using [`import_key()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a45e93f4a83143cb8ec6a09b67f91f4bb)**Return**

a map containing the private keys, indexed by their public key

#### [import\_key](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id16)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::import_key`\(string _account\_name\_or\_id_, string _wif\_key_\)  


Imports the private key for an existing account.

The private key must match either an owner key or an active key for the named account.

**See**

[dump\_private\_keys\(\)](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a98369ea6e10699066c7beb181996a219)**Return**

true if the key was imported**Parameters**

* `account_name_or_id`: the account owning the key
* `wif_key`: the private key in WIF format

#### [import\_accounts](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id17)

map&lt;string, bool&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::import_accounts`\(string _filename_, string _password_\)  


Imports accounts from a BitShares 0.x wallet file. Current wallet file must be unlocked to perform the import.

**Return**

a map containing the accounts found and whether imported**Parameters**

* `filename`: the BitShares 0.x wallet file to import
* `password`: the password to encrypt the BitShares 0.x wallet file

#### [import\_account\_keys](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id18)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::import_account_keys`\(string _filename_, string _password_, string _src\_account\_name_, string _dest\_account\_name_\)  


Imports from a BitShares 0.x wallet file, find keys that were bound to a given account name on the BitShares 0.x chain, rebind them to an account name on the 2.0 chain. Current wallet file must be unlocked to perform the import.

**Return**

whether the import has succeeded**Parameters**

* `filename`: the BitShares 0.x wallet file to import
* `password`: the password to encrypt the BitShares 0.x wallet file
* `src_account_name`: name of the account on BitShares 0.x chain
* `dest_account_name`: name of the account on BitShares 2.0 chain, can be same or different to `src_account_name`

#### [import\_balance](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id19)

vector&lt;signed\_transaction&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::import_balance`\(string _account\_name\_or\_id_, _const_ vector&lt;string&gt; &_wif\_keys_, bool _broadcast_\)  


This call will construct transaction\(s\) that will claim all balances controled by wif\_keys and deposit them into the given account.

**Parameters**

* `account_name_or_id`: name or ID of an account that to claim balances to
* `wif_keys`: private WIF keys of balance objects to claim balances from
* `broadcast`: true to broadcast the transaction on the network

#### [suggest\_brain\_key](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id20)

brain\_key\_info `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::suggest_brain_key`\(\)_const_  


Suggests a safe brain key to use for creating your account. [`create_account_with_brain_key()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1ac27928f7ca6db74e0ec4aee3ff0c545e) requires you to specify a ‘brain key’, a long passphrase that provides enough entropy to generate cyrptographic keys. This function will suggest a suitably random string that should be easy to write down \(and, with effort, memorize\).**Return**

a suggested brain\_key

#### [get\_transaction\_id](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id21)

transaction\_id\_type `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_transaction_id`\(_const_ signed\_transaction &_trx_\)_const_  


This method is used to convert a JSON transaction to its transactin ID.**Return**

the ID \(hash\) of the transaction**Parameters**

* `trx`: a JSON transaction

#### [get\_private\_key](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id22)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_private_key`\(public\_key\_type _pubkey_\)_const_  


Get the WIF private key corresponding to a public key. The private key must already be in the wallet.**Return**

the WIF private key**Parameters**

* `pubkey`: a public key in Base58 format

#### [load\_wallet\_file](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id23)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::load_wallet_file`\(string _wallet\_filename_ = ""\)  


Loads a specified Graphene wallet.

The current wallet is closed before the new wallet is loaded.

**Warning**

This does not change the filename that will be used for future wallet writes, so this may cause you to overwrite your original wallet unless you also call [`set_wallet_filename()`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#classgraphene_1_1wallet_1_1wallet__api_1aa5804e1ee29ff8f2c3bcc668ad2bfbcd)**Return**

true if the specified wallet is loaded**Parameters**

* `wallet_filename`: the filename of the wallet JSON file to load. If `wallet_filename` is empty, it reloads the existing wallet file

#### [normalize\_brain\_key](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id24)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::normalize_brain_key`\(string _s_\)_const_  


Transforms a brain key to reduce the chance of errors when re-entering the key from memory.

This takes a user-supplied brain key and normalizes it into the form used for generating private keys. In particular, this upper-cases all ASCII characters and collapses multiple spaces into one.**Return**

the brain key in its normalized form**Parameters**

* `s`: the brain key as supplied by the user

#### [save\_wallet\_file](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id25)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::save_wallet_file`\(string _wallet\_filename_ = ""\)  


Saves the current wallet to the given filename.

**Warning**

This does not change the wallet filename that will be used for future writes, so think of this function as ‘Save a Copy As…’ instead of ‘Save As…’. Use [`set_wallet_filename()`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#classgraphene_1_1wallet_1_1wallet__api_1aa5804e1ee29ff8f2c3bcc668ad2bfbcd) to make the filename persist.**Parameters**

* `wallet_filename`: the filename of the new wallet JSON file to create or overwrite. If `wallet_filename` is empty, save to the current filename.

