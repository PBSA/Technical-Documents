# Creating an Account

There are two ways to create  an account.

1. If you already have an account, that can be used to create another account.
2. Faucet

Method 1: With own funded account.







```text
def create_account(
    self,
    account_name,
    registrar=None,
    referrer="1.2.0",
    referrer_percent=50,
    owner_key=None,
    active_key=None,
    memo_key=None,
    password=None,
    additional_owner_keys=[],
    additional_active_keys=[],
    additional_owner_accounts=[],
    additional_active_accounts=[],
    proxy_account="proxy-to-self",
    storekeys=True,
    **kwargs
):
```

p.create\_account\("jemshid", "init0", "init0", 0, 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV', 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV', 'TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5S ...: o8BhtHuGYqET5GDW5CV'\)



```text
def transfer(self, to, amount, asset, memo="", account=None, **kwargs):
```

