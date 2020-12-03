# Setting up a Witness Node with Docker

## System Requirements

The following table lists what should be considered the recommended system requirements for running a **witness node on the Mainnet**:

| CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- |
| 4 Cores | 8GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |

The following table lists what should be considered the recommended system requirements for running a **witness node on SON:**

| **CPU** | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- |
| 2 Cores | 8GB | 60GB SSD | 1Gbps | Ubuntu 18.04 |

## Building with Docker

Clone the Peerplays Docker repository: [https://gitlab.com/data-security-node/peerplays-docker](https://gitlab.com/data-security-node/peerplays-docker):

```text
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/peerplays-docker
```

Use the install script to download/update the docker image for Peerplays:

```text
./run.sh install
```

Edit the configuration for the seed in `./data/witness_node_data_dir/seed_config.ini`

```text
vim ./data/witness_node_data_dir/seed_config.ini
```

{% hint style="info" %}
Optionally: you can edit the .env file in the root of the project to easily adjust parameters for running the project with Docker
{% endhint %}

```text
cp example.env .env
vim .env
```

{% hint style="warning" %}
The default setting will be to use the latest Peerplays \(Mainnet\) image. To change this, specify the DOCKER\_IMAGE variable
{% endhint %}

Start the witness node:

```text
./run.sh start
```

