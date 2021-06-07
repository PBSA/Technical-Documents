---
description: Setup a Witness Node using a pre-configured Docker container
---

# Witness Node Docker Install

This document assumes that you are running Ubuntu 18.04. Other Debian based releases may also work with the provided script.

The following steps outline the Docker installation of a Witness Node:

1. Preparing the Environment
2. Installing Docker
3. Starting the Container
4. Update the config.ini File
5. Create a Peerplays Account
6. Update config.ini with Witness Account Info
7. Start the Container and Vote for Yourself

## 1. Preparing the Environment

### 1.1. Hardware requirements

Please see the general Witness [hardware requirements](witness-node-hardware-requirements.md).

For the docker install, the requirements that we'll need for this guide would be as follows:

| Node Type? | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Witness | 8 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |

### 1.2. Installing the required dependencies

```text
sudo apt-get update
sudo apt-get install git curl
```

Then we'll clone the Peerplays Docker repository.

```text
git clone -b release https://gitlab.com/PBSA/tools-libs/peerplays-docker.git
```

## 2. Installing Docker

> **Note:** It is required to have Docker installed on the system that will be performing the steps in this document.

Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
sudo ./run.sh install_docker
```

Since the script has added the currently logged in user to the Docker group, you'll need to re-login \(or close and reconnect SSH\) for Docker to function correctly.

> **Note:** You can look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker. Or if you are having permission issues trying to run Docker, use `sudo` or look at [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

## 3. Start the Container

With at least 8GB of disk space available in your home folder, we'll run the docker container.

```text
sudo docker run -p 2001:2001 -p 9777:9777 -p 127.0.0.1:8090:8090 -v /dev/shm:/shm -v ~/peerplays-docker/data:/peerplays -d --name alice-seed -t datasecuritynode/peerplays:alice-0.1 witness_node --data-dir=/peerplays/witness_node_data_dir
```

Then we'll check the status of the container to see if all is well.

```text
sudo docker logs -f alice-seed
```

Last we'll stop the container so we can make updates to the config.ini file.

```text
sudo docker stop alice-seed
```

## 4. Update the config.ini File

We need to set the endpoint and seed-node addresses so we can access the cli\_wallet and download all the initial blocks from the chain. Within the config.ini file, locate the p2p-endpoint, rpc-endpoint, and seed-node settings and enter the following addresses.

```text
nano ~/peerplays-docker/data/witness_node_data_dir/config.ini

p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 0.0.0.0:8090
seed-node = 213.184.255.234:59500
```

Save the changes and start the container back up.

```text
sudo docker start alice-seed
```

## 5. Create a Peerplays Account

We'll need an account as the basis of creating a new Witness. The easiest way to do that is to use the GUI wallet.

### 5.1. Download the Peerplays Core GUI Wallet to Make an Account

[Peerplays Core GUI](https://github.com/peerplays-network/peerplays-core-gui/releases)

1. Install, open, and create an account. It's pretty self-explanatory. :\)
2. Wait for your node to sync the blocks \(about 7.3GB at the time of writing\). We need to do this before we can use the CLI wallet.
3. From this point on, please note the results of the following commands as you'll need them later.

### 5.2. Use the cli\_wallet to set a password and unlock the wallet

Back in the command line window, we can access the cli\_wallet program after all the blocks have been downloaded from the chain. Note that "your-password-here" is a password that you're creating for the cli\_wallet and doesn't necessarily have to be the password you used in the GUI wallet earlier.

```text
sudo docker exec -it alice-seed cli_wallet
set_password your-password-here
unlock your-password-here
```

The CLI wallet will show `unlocked >>>` when successfully unlocked.

> **Note:** A list of CLI wallet commands is available here: [https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls](https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls)

### 5.3. Generate OWNER private keys for the cli\_wallet and import them

This will return an array with your owner key in the form of \["PPYxxx", "xxxx"\]. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```text
get_private_key_from_password created-username owner created-password
```

The second value in the returned array is the private key of your owner key. Now we'll import that into the cli\_wallet.

```text
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

### 5.4. Generate ACTIVE private keys for the cli\_wallet and import them

Once again, this will return an array with your active key in the form of \["PPYxxx", "xxxx"\]. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```text
get_private_key_from_password created-username active created-password
```

The second value in the returned array is the private key of your active key. Now we'll import that into the cli\_wallet.

```text
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

> **NOTE:** The keys that begin with "PPY" are the public keys.

### 5.5. Upgrade to lifetime membership

You will need some PPY for this command to succeed. The account must have lifetime membership status to create a new Witness.

```text
upgrade_account created-username true
```

### 5.6. Create yourself as a Witness

The URL in this command is your own URL which should point to a page which describes who you are and why you want to become a Peerplays witness. Note your block signing key after you enter this command.

This command will require some PPY as well.

```text
create_witness created-username "https://your-url-to-witness-proposal" true
```

### 5.7. Gather your Witness account info

First we'll get the private key for your block\_signing\_key.

```text
get_private_key YOURBLOCKSIGNINGKEY
```

Then dump your keys to check and compare. One of the returned values from the following command should match your block\_signing\_key.

```text
dump_private_keys
```

Last we'll get your witness ID.

```text
get_witness created-username
```

## 6. Edit config.ini to include your Witness ID and your private key pair

Exit the cli\_wallet with the `quit` command. We'll stop the container and edit the config.ini file once again.

```text
sudo docker stop alice-seed
nano ~/peerplays-docker/data/witness_node_data_dir/config.ini

witness-id = "your_witness_id"
private-key = ["block_signing_key", "private_key_for_your_block_signing_key"]
```

## 7. Start the container and vote for yourself

```text
sudo docker start alice-seed
```

Once again, we need to wait for the node to sync the blocks to use the cli\_wallet. After the sync, you can vote for yourself.

```text
sudo docker exec -it alice-seed cli_wallet
unlock your-password-here
vote_for_witness created-username created-username true true
```

Now you can check your votes to verify it worked.

```text
get_witness your_witness_account
```

## 8. Glossary

**Witness:** An independent server operator which validates network transactions.

**Witness Node:** Nodes with a closed RPC port. They don't allow external connections. Instead these nodes focus on processing transactions into blocks.

