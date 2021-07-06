---
description: Configure your server to start your node on system boot-up
---

# Auto-Starting a Node

There are a few methods that can be used to start up a node on system boot-up. Automating the node to start when the server starts will help minimize downtime, allow the program to run in the background, and aid in making updates to the node software.

## 1. File Locations

In this tutorial it's assumed that your node was installed at `/usr/local/bin`. Please ensure the directories you use match your install. For example, programs in `/usr/local/bin` can be run without specifying the directory. But for the script to run programs located in other directories you'll need to specify the location explicitly, like `/home/ubuntu/src/peerplays/programs/witness_node/witness_node`.

For nodes installed with Docker, you'll simply need the location of the Docker shell script file \(`/home/ubuntu/peerplays-docker/run.sh`\).

## 2. Making a Shell Script

Making a shell script with logging is a good place to start. You'll be able to use this script to start up the node.

First make a log file to store the outputs of the `witness_node` program.

```text
sudo touch /var/log/peerplays.log
```

Find a good place to store the script file. For this tutorial, let's give it it's own directory. Then create a file named `start.sh`.

```text
cd /home/ubuntu
mkdir node_scripts
cd node_scripts
nano start.sh
```

Use the text editor of your choice \(nano comes with Ubuntu\) to create the `start.sh` file as follows \(please select the method which you used to install the node\):

{% tabs %}
{% tab title="Manual or GitLab Installed Nodes" %}
```text
#!/bin/bash

witness_node &> /var/log/peerplays.log
```

{% hint style="warning" %}
Depending on where the programs were installed, you might have to specify the file location explicitly. For example:

```text
#!/bin/bash

cd /home/ubuntu/src/peerplays
./programs/witness_node/witness_node &> /var/log/peerplays.log
```
{% endhint %}
{% endtab %}

{% tab title="Docker Installed Nodes" %}
```text
#!/bin/bash

cd /home/ubuntu/peerplays-docker
sudo ./run.sh start
```

{% hint style="info" %}
In the case of Docker, we don't have to output the logs to another file because we're already maintaining the logs. You can view them with:

```text
cd /home/ubuntu/peerplays-docker
sudo ./run.sh logs
```
{% endhint %}
{% endtab %}
{% endtabs %}

Save and exit the file. Now you'll set the file permissions.

```text
chmod 744 /home/ubuntu/node_scripts/start.sh
```

## 3. Auto-starting Methods

You'll only need to use **one** method to ensure your node starts at system boot. This tutorial will cover two options you can use:

* Using a system service with `Systemd`
* Using a cron job with `crontab`

### 3.1. System Service

Setting up a service using `Systemd` on Ubuntu is the preferred method of auto-starting your node. It allows for greater visibility of the status of the service. We'll make a service file that uses the shell script.

#### 3.1.1. Step 1: Make a System Service File

Now that you have the shell file good to go you'll create a service file. Navigate to `/etc/systemd/system` and create a file named `peerplays.service` as below.

```text
cd /etc/systemd/system
nano peerplays.service
```

Inside the `peerplays.service` file you'll enter:

```text
[Unit]
Description=Peerplays Node
After=network.target

[Service]
ExecStart=/home/ubuntu/node_scripts/start.sh
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Save the file and quit.

#### 3.1.2. Step 2: Enable the Service

```text
sudo systemctl enable peerplays.service
```

{% hint style="warning" %}
Make sure you don't get any errors.
{% endhint %}

```text
sudo systemctl status peerplays.service
```

If your node is running, stop it with `ctrl + c`, then start it back up with the service.

```text
sudo systemctl start peerplays.service
```

Lastly, check the log file to ensure the node is running properly.

```text
tail -f /var/log/peerplays.log
```

{% hint style="success" %}
Success!

You're all done if you've chosen to auto-start your node with systemd. No need for cron!
{% endhint %}

### 3.2. Cron Job

Cron jobs are simple to set up. If all you need is to ensure that your node starts when your system boots, a cron job is good enough.

#### 3.2.1. Step 1: Start up Crontab

```text
crontab -e
```

{% hint style="info" %}
If this is the first time you've used crontab on your machine, you'll be prompted to pick a text editor.
{% endhint %}

Crontab will open a file with some comments which explain how to configure a cron job. All you'll need to do is to specify the following at the end of the file:

```text
@reboot /home/ubuntu/node_scripts/start.sh
```

Save and quit the file. Now your script will execute whenever your system boots.

{% hint style="warning" %}
In some cases, the crond service needs to be enabled on boot for the configuration to function.

* To check if the crond service is enabled, use: `sudo systemctl status cron.service`
* To enable this service, use: `sudo systemctl enable cron.service`
{% endhint %}

{% hint style="success" %}
Success!

You're all done if you've chosen to auto-start your node with cron. No need for systemd!
{% endhint %}

## 4. List of Useful Systemd Commands

{% tabs %}
{% tab title="Services Management" %}
| Command | Description | Example |
| :--- | :--- | :--- |
| **`systemctl start <SERVICE>`** | Start a SERVICE \(not reboot persistent\) | `systemctl start peerplays.service` |
| **`systemctl stop <SERVICE>`** | Stop a SERVICE \(not reboot persistent\) | `systemctl stop peerplays.service` |
| **`systemctl restart <SERVICE>`** | Restart a SERVICE | `systemctl restart peerplays.service` |
| **`systemctl reload <SERVICE>`** | Reloads the configuration files without interrupting pending operations | `systemctl reload peerplays.service` |
| **`systemctl status <SERVICE>`** | Shows the status of a SERVICE | `systemctl status peerplays.service` |
| **`systemctl list-units --type=service`** | Displays the status of all services | n/a |
| **`systemctl list-unit-files --type=service`** | List the services that can be started or stopped | n/a |
| **`ls /etc/systemd/system/*.wants/`** | Print list of services \(alternate\) | n/a |
| **`systemctl enable <SERVICE>`** | Start SERVICE at next boot | `systemctl enable peerplays.service` |
| **`systemctl disable <SERVICE>`** | SERVICE won't be started at next boot | `systemctl disable peerplays.service` |
| **`systemctl is-enabled <SERVICE>`** | Check if a SERVICE is configured to start in the current environment | `systemctl is-enabled peerplays.service` |
| **`systemctl daemon-reload`** | Run this command after a change in any configuration file \(old or new\) | n/a |
| **`systemctl list-unit-files --type=service`** | List the services that can be started or stopped | n/a |
{% endtab %}

{% tab title="Investigation & Logs" %}
| Command | Description |
| :--- | :--- |
| **`journalctl -b`** | Show all messages from last boot |
| **`journalctl -b -p err`** | Show all messages of priority level ERROR and more from last boot |
| **`journalctl -f`** | Follow messages as they appear |
| **`journalctl -u <SERVICE>`** | Show logs for SERVICE |
| **`journalctl --full`** | Display all messages without truncating any |
| **`systemctl --state=failed`** | Display the services that failed to start |
| **`systemctl kill <SERVICE>`** | Gently kill the SERVICE |
| **`systemctl list-jobs`** | Show jobs |
{% endtab %}

{% tab title="System State" %}
| Command | Description |
| :--- | :--- |
| **`systemctl halt`** | Halts the system |
| **`systemctl poweroff`** | Powers off the system |
| **`systemctl reboot`** | Restarts the system |
| **`systemctl suspend`** | Suspends the system |
| **`systemctl hibernate`** | Hibernates the system |
| **`systemctl hybrid-sleep`** | Hibernates and suspends the system |
{% endtab %}
{% endtabs %}

## 5. Glossary

**Node:** The general term for the software that an independent server operator runs to perform some service for the network to which it belongs. In the case of Peerplays, that means validating network transactions, facilitating sidechain asset transfers, providing a gateway to on-chain data, or supplying / validating external data for dapps.

**System service \(Systemd\):** On Linux based systems \(Peerplays nodes require Ubuntu\), systemd is a system and service manager. In essence, it's an init system used to bootstrap user space and manage user processes. Systemd is the name of the program.

**Cron job \(crontab\):** A time-based job scheduler in Unix-like operating systems. Users who set up and maintain software environments use cron to schedule jobs \(commands or shell scripts\) to run periodically at fixed times, dates, or intervals. Crontab \(cron table\) is the file that cron uses to schedule tasks.

