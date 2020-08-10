# NFT command reference

### **Step: 1** Create Metadata

**Command used** : `nft_metadata_create <<account_name>> <<metadata_name>> <<metadata_symbol>> <<base_uri>> true true true`

**For example :** `nft_metadata_create account01 sknft sknft sknft null null true true true`  


### **Step:2** Update Metadata

**Command used** : `nft_metadata_update <<account_name>> <<metadata_id>> <<new_name>> <<new_symbol>> <<new_base_uri>> null null true true true`

**For Example :**`nft_metadata_update account01 1.30.1 sknft01 sknft01 sknft01 null null true true true`  
  


### **Step: 3** Create NFT

**Command used** : `nft_create <<account_name>> <<metadata_id>> <<Owner_account_name>> <<approve_aacount_name>> <<token_uri>> true`

**For Example** : `nft_create account01 1.30.0 account03 account03 sknftmint true`  


### **Step:4** Get NFT balance

**Command used** : `nft_get_balance <<account_name>>`

**For Example :** `nft_get_balance account01`  
  


### **Step:5** To verify the owner of created NFT

**Command used** : `nft_owner_of <<nft_id>>`

**For Example :** `nft_owner_of 1.31.1`  
  


### **Step:6** Safe NFT transfer

**Command Used :** `nft_safe_transfer_from <<Operator_account_name>> <<transfer_from_account_name>> <<transfer_to_account_name>> <<nft_id>> true true`

**For Example** : `nft_safe_transfer_from account01 account01 aaccount02 1.31.1 true true`  
  


### **Step: 7** NFT Transfer

**Command used :** `nft_transfer_from <<Operator_account_id>> <<From_account_id>> <<To_account_id>> <<nft_id>> true`

**For Example** : `nft_transfer_from 1.2.31 1.2.31 1.2.28 1.31.37 true`  
  


### **Step:8** NFT Approve

**Command used** : `nft_approve <<new_operator_account_id>> <<new_account_id>> <<ndt_id>> true`

**For Example** : `nft_approve 1.2.19 1.2.19 1.31.1 true`  
  


### **Step : 9** To approve all NFTs at once

**Command used:** `nft_set_approval_for_all <<Owner_account_id>> <<Operator_account_id>> true true`

**For example** : `nft_set_approval_for_all 1.2.21 1.2.21 true true`  
  


### **Step: 10** To see the approved account details

**Command used** : `nft_get_approved <<approve_nft_id>>`

**For Example:** `nft_get_approved 1.31.0`  
  


### **Step:11** Approved for all

**Command used:** `nft_is_approved_for_all <<owner_account_id>> <<operator_account_id>>`

**For Example :** `nft_is_approved_for_all 1.2.21 1.2.21`  
  


### **Step :12** Get list of all created NFT

**Command used** : `nft_get_all_tokens`

With this - all the created NFTs are listed on the machine.

