---
description: Setup a Witness Node using a pre-configured Docker container
---

# Docker Install

This document assumes that you are running Ubuntu 18.04. Other Debian based releases may also work with the provided script.

The following steps outline the Docker installation of a Witness Node:

1. Preparing the Environment
2. Installing Docker
3. Installing the Peerplays image
4. Starting the Container
5. Update the `config.ini` File
6. Create a Peerplays Account
7. Update `config.ini` with Witness Account Info
8. Start the Container and Vote for Yourself

## 1. Preparing the Environment

### 1.1. Hardware requirements

Please see the Witness node [hardware requirements](../../the-basics/hardware-requirements.md).

For the docker install on Peerplays **Mainnet**, the requirements that we'll need for this guide would be as follows \(as per the hardware requirements doc\):

| Node Type? | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Witness | 4 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |

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

{% hint style="info" %}
It is required to have Docker installed on the system that will be performing the steps in this document.
{% endhint %}

Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
sudo ./run.sh install_docker
```

The `run.sh` script contains many commands to make managing the node easy. A list of all its [commands](witness-docker-install.md#commands-list) are listed in section 9 of this document.

Since the script has added the currently logged in user to the Docker group, you'll need to re-login \(or close and reconnect SSH\) for Docker to function correctly.

{% hint style="info" %}
You can look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker. Or if you are having permission issues trying to run Docker, use `sudo` or look at [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/).
{% endhint %}

### 2.1. Setting up the .env file

Copy the `example.env` to `.env` located in the root of the repository:

```text
cd ~/peerplays-docker
cp example.env .env
```

We're going to have to make some changes to the `.env` file so we'll open that now using a text editor.

```text
nano .env
```

Here are the important parts of the `.env` file. These will be the parts that need to be edited or optionally edited. The rest of the file should be unchanged.

```text
# Comma separated port numbers to expose to the internet (binds to 0.0.0.0)
# Expose 9777 to the internet, but only expose RPC ports 8090 and 8091 onto 127.0.0.1 (localhost)
# allowing the host machine access to the container's RPC ports via 127.0.0.1:8090 and 127.0.0.1:8091
# We'll need ports 8090 and 8091 open to our localhost to interact with the Peerplays CLI Wallet.
PORTS=9777,127.0.0.1:8090:8090,127.0.0.1:8091:8091

# Websocket RPC node to use by default for ./run.sh remote_wallet
REMOTE_WS=""
```

## 3. Installing the Peerplays image

Use `run.sh` to pull the node image:

```text
cd ~/peerplays-docker
sudo ./run.sh install
```

## 4. Start the Container

With at least 8GB of disk space available in your home folder, we'll start the node. This will create and/or start the Peerplays docker container.

```text
sudo ./run.sh start
```

Then we'll check the status of the container to see if all is well.

```text
sudo ./run.sh logs
```

Last we'll stop the container so we can make updates to the `config.ini` file.

```text
sudo ./run.sh stop
```

## 5. Update the config.ini File

We need to set the endpoint and seed-node addresses so we can access the cli\_wallet and download all the initial blocks from the chain. Within the `config.ini` file, locate the p2p-endpoint, rpc-endpoint, and seed-node settings and enter the following addresses.

```text
nano ~/peerplays-docker/data/witness_node_data_dir/config.ini

p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 127.0.0.1:8090
seed-node = 213.184.255.234:59500
```

Save the changes and start the container back up.

```text
sudo ./run.sh start
```

## 6. Create a Peerplays Account

We'll need an account as the basis of creating a new Witness. The easiest way to do that is to use the GUI wallet.

### 6.1. Download the Peerplays Core GUI Wallet to Make an Account

[Peerplays Core GUI](https://github.com/peerplays-network/peerplays-core-gui/releases)

1. Install, open, and create an account. It's pretty self-explanatory. 🙂 
2. Wait for your node to sync the blocks \(about 7.3GB at the time of writing\). We need to do this before we can use the CLI wallet.
3. From this point on, please note the results of the following commands as you'll need them later.

### 6.2. Use the cli\_wallet to set a password and unlock the wallet

Back in the command line window, we can access the cli\_wallet program after all the blocks have been downloaded from the chain. Note that "your-password-here" is a password that you're creating for the cli\_wallet and doesn't necessarily have to be the password you used in the GUI wallet earlier.

```text
sudo ./run.sh wallet
set_password your-password-here
unlock your-password-here
```

The CLI wallet will show `unlocked >>>` when successfully unlocked.

{% hint style="info" %}
A list of CLI wallet commands is available here: [https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls](https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls)
{% endhint %}

### 6.3. Generate OWNER private keys for the cli\_wallet and import them

This will return an array with your owner key in the form of `["PPYxxx", "xxxx"]`. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```text
get_private_key_from_password created-username owner created-password
```

The second value in the returned array is the private key of your owner key. Now we'll import that into the cli\_wallet.

```text
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

### 6.4. Generate ACTIVE private keys for the cli\_wallet and import them

Once again, this will return an array with your active key in the form of `["PPYxxx", "xxxx"]`. Note that the "created-username" and "created-password" used here are the username and password from the GUI wallet!

```text
get_private_key_from_password created-username active created-password
```

The second value in the returned array is the private key of your active key. Now we'll import that into the cli\_wallet.

```text
import_key "created-username" SECONDVALUEFROMLASTCOMMAND
```

{% hint style="info" %}
The keys that begin with "PPY" are the public keys.
{% endhint %}

### 6.5. Upgrade to lifetime membership

You will need some PPY for this command to succeed. The account must have lifetime membership status to create a new Witness.

```text
upgrade_account created-username true
```

### 6.6. Create yourself as a Witness

The URL in this command is your own URL which should point to a page which describes who you are and why you want to become a Peerplays witness. Note your block signing key after you enter this command.

This command will require some PPY as well.

```text
create_witness created-username "https://your-url-to-witness-proposal" true
```

### 6.7. Gather your Witness account info

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

## 7. Edit config.ini to include your Witness ID and your private key pair

Exit the cli\_wallet with the `quit` command. We'll stop the container and edit the `config.ini` file once again.

```text
sudo ./run.sh stop
nano ~/peerplays-docker/data/witness_node_data_dir/config.ini

witness-id = "your_witness_id"
private-key = ["block_signing_key", "private_key_for_your_block_signing_key"]
```

## 8. Start the container and vote for yourself

```text
sudo ./run.sh start
```

Once again, we need to wait for the node to sync the blocks to use the cli\_wallet. After the sync, you can vote for yourself.

```text
sudo ./run.sh wallet
unlock your-password-here
vote_for_witness created-username created-username true true
```

Now you can check your votes to verify it worked.

```text
get_witness your_witness_account
```

{% hint style="success" %}
Complete! You've installed your Witness node and you're up and running. 🎊 
{% endhint %}

## 9. What's Next?

### 9.1. Auto-starting your node

Up until this point we have been running the node in the foreground which is fragile and inconvenient. So let's start the node as a service when the system boots up instead.

{% page-ref page="../../the-basics/auto-starting-a-node.md" %}

### 9.2. Creating a backup node

After that, it would be smart to create a backup server to enable you to make software updates, troubleshoot issues with the node, and otherwise take your node offline without causing service outages.

{% page-ref page="../../the-basics/backup-servers.md" %}

### 9.3. Join the Testnet \(Beatrice\)

As we all know, testing should never be done in production. This is why all node operators must also participate in the public testnet, Beatrice.

{% page-ref page="../../the-basics/joining-the-public-testnet.md" %}

### 9.4. Fire up another node 🔥 

You've got a Witness node. Now you'll need a BOS node. And since you're in the node making mood, how about a SON too?

{% hint style="warning" %}
When you make any node, don't forget [Beatrice](../../the-basics/joining-the-public-testnet.md)!
{% endhint %}

### 9.5. Enable SSL to encrypt your node traffic

If you have a node that is accessible from the internet \(for example, an API or Seed node\) it would be wise to enable SSL connections to your node.

{% page-ref page="../../advanced-topics/reverse-proxy-for-enabling-ssl.md" %}

## 10. Docker `run.sh` commands list

* `start` - starts seed container
* `start_son` - starts son seed container
* `start_son_regtest` - starts son seed container and bitcoind container under the docker network
* `clean` - Remove blockchain, p2p, and/or shared mem folder contents, seed, bitcoind, and son docker network \(warns beforehand\)
* `dlblocks` - download and decompress the blockchain to speed up your first start
* `replay` - starts seed container \(in replay mode\)
* `replay_son` - starts son seed container \(in replay mode\)
* `memory_replay` - starts seed container \(in replay mode, with --memory-replay\)
* `shm_size` - resizes /dev/shm to size given, e.g. ./run.sh shm\_size 10G
* `stop` - stops seed container
* `status` - show status of seed container
* `restart` - restarts seed container
* `install_docker` - install docker
* `install` - pulls latest docker image from server \(no compiling\)
* `install_full` - pulls latest \(FULL NODE FOR RPC\) docker image from server \(no compiling\)
* `rebuild` - builds seed container \(from docker file\), and then restarts it
* `build` - only builds seed container \(from docker file\)
* `logs` - show all logs inc. docker logs, and seed logs
* `wallet` - open cli\_wallet in the container
* `remote_wallet` - open cli\_wallet in the container connecting to a remote seed
* `enter` - enter a bash session in the currently running container
* `shell` - launch the seed container with appropriate mounts, then open bash for inspection

## 11. Glossary

**Witness:** An independent server operator which validates network transactions.

**Witness node:** Nodes with a closed RPC port. They don't allow external connections. Instead these nodes focus on processing transactions into blocks.

