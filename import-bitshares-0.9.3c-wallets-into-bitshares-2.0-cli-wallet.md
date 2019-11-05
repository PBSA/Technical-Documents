# Import-BitShares-0.9.3c-wallets-into-BitShares-2.0-CLI-wallet

In bitshares 0.9.3c, run:

```text
wallet_export_keys /tmp/final_bitshares_keys.json
```

in bitshares 2.0 CLI wallet, run:

> > > import\_accounts /tmp/final\_bitshares\_keys.json my\_password

then, for each account in your wallet \(run list\_my\_accounts to see them\):

> > > import\_account\_keys /tmp/final\_bitshares\_keys.json my\_password my\_account\_name my\_account\_name

note: in the release tag, this will create a full backup of the wallet after every key it imports. If you have thousands of keys, this is quite slow and also takes up a lot of disk space. Monitor your free disk space during the import and, if necessary, periodically erase the backups to avoid filling your disk. The latest code only saves your wallet after all keys have been imported.

> > > import\_balance my\_account\_name \["\*"\] true list\_account\_balances my\_account\_name

Verify the Results

