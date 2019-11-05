# Wallet-Server-Architecture

To maintain a user experience similar to any other website, it is critical that users can log into the website and gain access to their private keys from any computer. It is equally important that the server does not have access to the user's private keys.

The goal of this specification is to define a protocol between the web browser and the server for storing and retrieving private data of the user. Because all data is encrypted, the server will need to use some techniques to prevent abuse. We will assume that prior to storing data on the server the user must request a code that will be sent to them by email.

The syntax below is shorthand for JSON-RPC requests sent to/from the server:

```text
server.requestCode( email )
```

Then the user can store data on the server like so:

```text
server.createWallet( code, encrypted_data, signature ) 
```

The wallet will be saved on the server using the _public\_key_ derived from _encrypted\_data_ and _signature_. The code contains a irreversible hash of the email. This is used in a unique constraint effectively limiting users to one wallet per email. The original email address is not stored in the database.

Then the user can fetch data from the server like so:

```text
server.fetchWallet( publickey, local_hash )
```

This method will only fetch the wallet if it is different than what is already cached locally. In addition to returning the encrypted wallet, this method will also return statistics about when it was last updated, and which IPs have requested the wallet and when. This can be used by the user to detect potential attempts at fraud.

The user can update their data with this call:

```text
server.saveWallet( original_local_hash, encrypted_data, signature )
```

The signature can be used to derive the public key under which encrypted\_data should be stored. The server will have to verify that derived public key exists in the database or this method will fail. If the original\_local\_hash does not match the wallet being overwritten this method needs to fail.

The user can change their "key" with the following call:

```text
server.changePassword( original_local_hash, original_signature, new_encrypted_data, new_signature )
```

After this call the public key used to lookup this wallet will be the one derived from _new\_signature_ and _new\_encrypted\_data_. A wallet must exist at the _old\_public\_key_ derived from the _original\_local\_hash_ and _original\_signature_.

The user may delete their wallet with the following call:

```text
server.deleteWallet( local_hash, signature )
```

For security reasons, this delete is permanent. The user may repeat the process to create a new wallet using the same email.

## Generating Public Keys

Users generate a private key from their password. Generally speaking, passwords are too weak and could be strengthened by being combined with other information that is both unique to the user and unlikely for the user to forget. At a minimum email address and/or username can be combined with the password to recover the private key for the wallet. Additional information such as social security numbers, passport IDs, phone numbers, or answers to other security questions may be useful for some applications. Keep in mind that the user must remember both their password AND the exact set of security Questions **and** Answers in order to recover their account.

## Mitigating Brute Force Attacks

The server should limit the number of wallet requests it accepts per IP address to a fixed number per hour. In this way users wallet data is kept secure from attempts at brute force attacks unless the server itself is compromised. In the event the server is compromised, users are only protected by the quality of their password and any other data used as salt.

## Saving the Wallet Locally

The browser should cache the wallet data locally

