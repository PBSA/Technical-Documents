# HRP / RBAC

### Operations



#### Create Custom Permission

```text
p.custom_permission_create(
    permission_name,
    owner_account=None,
    weight_threshold=[],
    account_auths=[],
    key_auths=[],
    address_auths=[],
    )
```

For example 

```text
    p.custom_permission_create(
        testperm1,
        owner_account="1.2.7",
        weight_threshold=1,
        account_auths=[["1.2.8", 1]])
```



#### Custom Permission Update

```text
p.custom_permission_update(
    permission_id,
    owner_account=None,
    weight_threshold=[],
    account_auths=[],
    key_auths=[],
    address_auths=[],
    )
```

For example

```text
            p.custom_permission_update(
                    permission_id,
                    weight_threshold=1,
                    account_auths=[["1.2.9", 2]],
                    owner_account="1.2.7"
            )
```



#### Custom Permission Delete

```text
p.custom_permission_delete(
    permission_id,
    owner_account=None,
    )
```

For example

```text
            p.custom_permission_delete(
                    permission_id,
                    owner_account="1.2.7"
            )
```



#### Custom Account Authority Create

```text
p.custom_account_authority_create(
    permission_id,
    operation_type,
    valid_from,
    valid_to,
    owner_account=None,
    )
```

For example

```text
p.custom_account_authority_create(
                    permission_id,
                    0,
                    "2020-07-27T00:00:00",
                    "2030-07-27T00:00:00",
                    owner_account="1.2.7") 
```



#### Custom Account Authority Update

```text
p.custom_account_authority_update(
    auth_id,
    new_valid_from,
    new_valid_to,
    owner_account=None,
    )
```

For example

```text
p.custom_account_authority_update(
            authority_id,
            "2020-07-27T00:00:00",
            "2040-07-27T00:00:00",
            owner_account="1.2.7")
```



#### Custom Account Authority Delete

```text
p.custom_account_authority_delete(
    auth_id,
    owner_account=None,
    )
```

For example

```text
    p.custom_account_authority_delete(
            authority_id,
            owner_account="1.2.7")
```



### RPC Info calls

```text
get_custom_permissions(account)
get_custom_permission_by_name(account, permission_name)
get_custom_account_authorities(account)
get_custom_account_authorities_by_permission_id(permission_id)
get_custom_account_authorities_by_permission_name(account, permission_name)
get_active_custom_account_authorities_by_operation(account, int operation_type)
```

