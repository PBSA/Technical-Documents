# Setting up a Witness Node

This is an introduction for new Witnesses to get up to speed on the Peerplays blockchain. It is intended for Witnesses planning to join a live, already deployed, blockchain.

The following repository should be used in support of this document:

{% embed url="https://github.com/peerplays-network/peerplays" %}

## System Requirements

The following table lists what should be considered the minimum system requirements for running a witness node:

| CPU     | Memory | Storage   | Bandwidth | OS           |
| ------- | ------ | --------- | --------- | ------------ |
| 8 Cores | 64GB   | 300GB SSD | 1Gbps     | Ubuntu 18.04 |

These requirements are as of the time of writing, so consider deploying a server with specs slightly higher than the ones listed above in order to "future proof" your server in case the minimum requirements grow in the future.

## Building on Ubuntu 18.04 LTS and Installation Instructions

[https://infra.peerplays.tech/witnesses/installation-guides/manual-install](https://infra.peerplays.tech/witnesses/installation-guides/manual-install)

## Installing using Docker&#x20;

This Docker image can be installed as an alternative to the previous steps. It doesn't need to be run if those steps have already been completed.



[https://infra.peerplays.tech/witnesses/installation-guides/docker-install](https://infra.peerplays.tech/witnesses/installation-guides/docker-install)

##

### Upgrading A Peerplays Witness Node

To minimize downtime of your Peerplays Witness node when upgrading, it's recommended to create a backup Witness server.

{% content-ref url="creating-a-backup-server.md" %}
[creating-a-backup-server.md](creating-a-backup-server.md)
{% endcontent-ref %}

## CLI Wallet Setup

The next step is to set up the CLI Wallet.

{% content-ref url="cli-wallet-setup.md" %}
[cli-wallet-setup.md](cli-wallet-setup.md)
{% endcontent-ref %}

## Auto-Starting the Witness Node

It's important for your Witness node to start when your system boots up. The `filepaths` here assume that you installed your witness into `/home/ubuntu/peerplays`

Step 1. Create a log file to hold your `stdout/err` logging

```
sudo touch /var/log/peerplays.log
```

Step 2. Save this file in your Peerplays directory. `vi /home/ubuntu/peerplays/start.sh`

```
#!/bin/bash

cd /home/ubuntu/peerplays
./programs/witness_node/witness_node &> /var/log/peerplays.log
```

Step 3. Make it executable

```
chmod 744 /home/ubuntu/peerplays/start.sh
```

Step 4. Create this file: `sudo vi /etc/systemd/system/peerplays.service`

{% hint style="warning" %}
**Note**: Check the path for `start.sh`, if necessary, change it to match where your `start.sh` file actually is.
{% endhint %}

```
[Unit]
Description=Peerplays Witness
After=network.target

[Service]
ExecStart=/home/ubuntu/peerplays/start.sh

[Install]
WantedBy = multi-user.target
```

Step 5. Enable the service

```
sudo systemctl enable peerplays.service
```

{% hint style="danger" %}
**Important**: Make sure you don't get any errors.
{% endhint %}

```
sudo systemctl status peerplays.service
```

Step 6. Stop your Witness node, if it's currently running, then start it with the service.

```
sudo systemctl start peerplays.service
```

Step 7. Check your `logfile` for entries

```
tail -f /var/log/peerplays.log
```

## BOS and MINT Setup

All Witnesses are also required to install and run the Bookie Oracle Suite (BOS) and the supporting manual intervention tool (MINT).

{% content-ref url="../../bookie-oracle-suite-bos/bos-and-mint-setup/" %}
[bos-and-mint-setup](../../bookie-oracle-suite-bos/bos-and-mint-setup/)
{% endcontent-ref %}
