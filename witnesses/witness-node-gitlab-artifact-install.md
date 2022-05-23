---
description: Setup a Witness Node using a pre-compiled GitLab artifact
---

# Witness Node GitLab Artifact Install

This document assumes that you are running Ubuntu 18.04.

The following steps outline the artifact installation of a Witness Node:

1. Prepare the environment
2. Download and extract the artifacts
3. Copy the artifacts to the proper locations
4. Update the config.ini File
5. Create a Peerplays Account
6. Update config.ini with Witness Account Info
7. Start the Witness and Vote for Yourself
8. Auto-Starting the Witness Node

{% hint style="info" %}
Before we begin, to set up a Witness node requires about 15 PPY. This is to pay for an upgraded account (5 PPY) and to create a new witness (8 PPY). The remaining funds are to pay for various transaction fees while setting up the node (like voting for yourself!).

Note that these fees will likely change over time as recommended by the Committee of Advisors.
{% endhint %}

## 1. Prepare the environment

### 1.1. Hardware requirements

Please see the general Witness [hardware requirements](https://app.gitbook.com/@peerplays/s/documents/witnesses/witness-node-hardware-requirements).

For the GitLab artifact install, the requirements that we'll need for this guide would be as follows:

| Node Type? | CPU     | Memory | Storage   | Bandwidth | OS           |
| ---------- | ------- | ------ | --------- | --------- | ------------ |
| Witness    | 8 Cores | 16GB   | 100GB SSD | 1Gbps     | Ubuntu 18.04 |

> **NOTE:** The artifacts from GitLab are already built for x86\_64 architecture. These will not work with ARM based architecture.

### 1.2. Install the dependencies

The following dependencies are necessary for a clean install of Ubuntu 18.04 LTS:

```
sudo apt-get update
sudo apt-get -y  install libzmq3-dev wget zip unzip
```

## 2. Download and extract the artifacts

Artifacts are pre-built binaries that are available to download from GitLab. You can see the available pipelines, sorted by release tags, on the GitLab [Peerplays project](https://gitlab.com/PBSA/peerplays/-/pipelines?scope=tags\&page=1) page. The link in the code below refers to release tag `1.5.11` which is the latest production release as of the writing of this document. Please make sure to replace the tag with the one you need. The test releases are available as well, `test-1.5.5` for example.

```
cd /home/ubuntu
mkdir artifacts
cd artifacts
sudo wget https://gitlab.com/PBSA/peerplays/-/jobs/artifacts/1.5.11/download?job=build -O artifacts.zip
unzip artifacts.zip
```

> **Important:** Double check the tag in the download link!

## 3. Copy the artifacts to the proper locations

Putting the witness\_node and cli\_wallet programs in the `/usr/local/bin` directory will allow us to call the program from any directory.

```
sudo cp /home/ubuntu/artifacts/build/programs/witness_node/witness_node /usr/local/bin/witness_node
sudo cp /home/ubuntu/artifacts/build/programs/cli_wallet/cli_wallet /usr/local/bin/cli_wallet
```

Now we can run start the node with:

```
witness_node
```

Launching the witness creates the required directories which contain the config.ini file we'll need to edit. We'll stop the witness now with `Ctrl + C` so we can edit the config file.

## 4. Edit the config.ini file

We need to set the endpoint and seed-node addresses so we can access the cli\_wallet and download all the initial blocks from the chain. Within the config.ini file, locate the p2p-endpoint, rpc-endpoint, and seed-node settings and enter the following addresses.

```
nano ~/witness_node_data_dir/config.ini

p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 127.0.0.1:8090
seed-node = 213.184.255.234:59500
```

Save the changes and start the Witness back up.

```
witness_node
```

## 5. Create a Peerplays Account

We'll need an account as the basis of creating a new Witness. The easiest way to do that is to use the GUI wallet.

### 5.1. Download the Peerplays Core GUI Wallet to Make an Account

[Peerplays Core GUI](https://github.com/peerplays-network/peerplays-core-gui/releases)

1. Install, open, and create an account. It's pretty self-explanatory. :)
2. Wait for your node to sync the blocks (about 7.3GB at the time of writing). We need to do this before we can use the CLI wallet.
3. From this point on, please note the results of the following commands as you'll need them later.

### 5.2. Use the cli\_wallet to set a password and unlock the wallet

Back in a **new** command line window, we can access the cli\_wallet program after all the blocks have been downloaded from the chain. Note that "your-password-here" is a password that you're creating for the cli\_wallet and doesn't necessarily have to be the password you used in the GUI wallet earlier.

```
cli_wallet
set_password your-password-here
unlock your-password-here
```

The CLI wallet will show `unlocked >>>` when successfully unlocked.

> **Note:** A list of CLI wallet commands is available here: [https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls](https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls)

### 5.3. Generate OWNER private keys for the cli\_wallet and import them

This will return an array with your owner key in the form of \["PPYxxx", "xxxx"]. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```
get_private_key_from_password created-username owner created-password
```

The second value in the returned array is the private key of your owner key. Now we'll import that into the cli\_wallet.

```
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

### 5.4. Generate ACTIVE private keys for the cli\_wallet and import them

Once again, this will return an array with your active key in the form of \["PPYxxx", "xxxx"]. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```
get_private_key_from_password created-username active created-password
```

The second value in the returned array is the private key of your active key. Now we'll import that into the cli\_wallet.

```
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

> **NOTE:** The keys that begin with "PPY" are the public keys.

### 5.5. Upgrade to lifetime membership

You will need some PPY for this command to succeed. The account must have lifetime membership status to create a new Witness.

```
upgrade_account created-username true
```

### 5.6. Create yourself as a Witness

The URL in this command is your own URL which should point to a page which describes who you are and why you want to become a Peerplays witness. Note your block signing key after you enter this command.

This command will require some PPY as well.

```
create_witness created-username "https://your-url-to-witness-proposal" true
```

### 5.7. Gather your Witness account info

First we'll get the private key for your block\_signing\_key.

```
get_private_key YOURBLOCKSIGNINGKEY
```

Then dump your keys to check and compare. One of the returned values from the following command should match your block\_signing\_key.

```
dump_private_keys
```

Last we'll get your witness ID.

```
get_witness created-username
```

## 6. Edit config.ini to include your Witness ID and your private key pair

Exit the cli\_wallet with the `quit` command. We'll stop the Witness (`Ctrl + C` in the first command line window) and edit the config.ini file once again.

```
nano ~/witness_node_data_dir/config.ini

witness-id = "your_witness_id"
private-key = ["block_signing_key", "private_key_for_your_block_signing_key"]
```

## 7. Start the Witness and vote for yourself

```
witness_node
```

Once again, we need to wait for the node to sync the blocks to use the cli\_wallet. After the sync, you can vote for yourself. Back in the second command line window:

```
cli_wallet
unlock your-password-here
vote_for_witness created-username created-username true true
```

Now you can check your votes to verify it worked.

```
get_witness your_witness_account
```

## 8. Auto-Starting the Witness Node

Up until this point we have been running the node in the foreground which is fragile and inconvenient. So let's start the node as a service when the system boots up instead.

Step 1. Create a log file to hold your `stdout/err` logging

```
sudo touch /var/log/peerplays.log
```

Step 2. Save this file in a Peerplays directory. `nano /home/ubuntu/peerplays/start.sh`

```
#!/bin/bash

witness_node &> /var/log/peerplays.log
```

Step 3. Make it executable

```
chmod 744 /home/ubuntu/peerplays/start.sh
```

Step 4. Create this file: `sudo nano /etc/systemd/system/peerplays.service`

```
[Unit]
Description=Peerplays Witness
After=network.target

[Service]
ExecStart=/home/ubuntu/peerplays/start.sh
Restart=always
RestartSec=3

[Install]
WantedBy = multi-user.target
```

Step 5. Enable the service

```
sudo systemctl enable peerplays.service
sudo systemctl status peerplays.service
```

> Make sure you don't get any errors.

Step 6. Stop your Witness node, if it's currently running, then start it with the service.

```
sudo systemctl start peerplays.service
```

Step 7. Check your log file for entries

```
tail -f /var/log/peerplays.log
```
