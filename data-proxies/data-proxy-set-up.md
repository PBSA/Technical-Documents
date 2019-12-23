# Data Proxy Set Up

Data proxies are a concept introduced in Peerplays blockchain to anonymize input data feeds. For now we are using the same to anonymize sports data feeds in a decentralized manner. This document provides setup instructions.

### Instructions <a id="Data-Proxy-setup-how-to-Instructions"></a>

We are assuming Ubuntu 18.04 LTS and Python3.X available by default as the versions to be supported.

Prepare the servers

* Update and upgrade the server with the correct packages

| `apt update ; apt install` `-y gunicorn mongodb-server virtualenv python3 python3-dev git htop mosh openssl libssl-dev` |
| :--- |


* Get the code

| `git clone git@github.com:PBSA/bos-dataproxy-legacy.git` |
| :--- |


The data proxy configuration file should be at the top level foder, ie bos-dataproxy-legacy. The sample file  **`config-example.yaml`** can be copied to **`config-dataproxy.yam`**l

* Install the necessary package by running `run_dev_server.sh`. This calls `setup.s`h internally to assemble everything.

| `cd` `bos-dataproxy-legacy; source` `run_dev_server.sh` |
| :--- |


PS: for production this will be run\_production\_server.sh

These steps will install the necessary packages. Now for every feed provider, we will have to use the Provider functionality and write wrappers to fetch the code. Once the code is fetched, this will be normalized and passed on to the BOS component.

Running the dev environment using the following command starts the general environment.

| `source` `provider_service.sh` |
| :--- |


For example, using **Scorespro** as a provider:

make sure that the configuration file `config-dataproxy.yaml` is in the working directory and run the following command. The above script is a wrapper. Example configuration file is provided.

| `python provider_service.py scorespro run_here` |
| :--- |


result:

| `(env) root@208e204975be:~/work/code/BOS/l/bos-dataproxy-legacy# python provider_service.py scorespro run_hereSetting up logger handling for dataproxy...... done2019-08-07 16:51:50,560 INFO       dataproxy: Custom config has been loaded from working directory: config-dataproxy.yaml2019-08-07 16:51:50,708 INFO       bos_incidents: Custom config has been loaded ;config-defaults.yaml;incident_storage_config.yamlSetting up logger handling for dataproxy...... done2019-08-07 16:52:03,106 INFO       dataproxy.provider.scorespro.pap_task: Sport SOC found and added to history watch` |
| :--- |


### How is data fetched from a provider ? <a id="Data-Proxy-setup-how-to-Howisdatafetchedfromaprovider?"></a>

We have code available for a few other data feed providers which is available on request.

For additional providers, we need to pull or receive push data and then write a wrapper to handle it.

### How to start the services as a daemon <a id="Data-Proxy-setup-how-to-Howtostarttheservicesasadaemon"></a>

We can use the `screen` command to start the above commands in daemon mode.

### Nginx Reverse Proxy <a id="Data-Proxy-setup-how-to-NginxReverseProxy"></a>

To be added. Not mandatory unless the status needs to be published.

