# EXEBlock Knowledge Base : How to create Sidechain node docker container

\*\*NOTE: See attached Dockerfile  
\*\*NOTE: This setup is for the sidechain node which works with the latest sidechain blockchain repo and builds the witness node and wallet cli.

INSTRUCTIONS:

1. **Using the docker preferences option, go to the Advanced tab and increase the memory of the virtual machine to minimum 4Gb.**  This is necessary for running the container.
2. **Navigate to the directory where the file is, and build the docker:** _docker build -t exeblock/sidechain-node --no-cache ._ _Additional scripts made from CMD of building the docker container are shown below_
3.  **When completed, run the docker:** _docker run -dit --name sidechain-node -p 9777:9777 -p 8090:8090 exeblock/sidechain-node /bin/bash The docker container is now running with a witness node in the background and the cli wallet for you to interact with._
4. **\(To debug and test\) Connect with a separate console:** _docker exec -it sidechain-node /bin/bash_
5. **Within this container from the docker exec you can run the scripts**\(sh wallet\_node.sh\) **or go through the setups from the How-To:** __[_How to setup a Private Testnet of Peerplays/5050_](exeblock-knowledge-base-how-to-setup-a-private-testnet-of-peerplays-5050.md)

**Dockerfile**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p># This will build the witness_node in a docker image. This was based off
          of the work by
          <br /># Alex Morgulis</p>
        <p>FROM ubuntu:16.04</p>
        <p># default is 8090 unless a --build-args SIDE_PORT=1337 is added on docker
          build.
          <br />ENV SIDE_PORT 8090
          <br />EXPOSE $SIDE_PORT</p>
        <p>ENV P2P_PORT 9777
          <br />EXPOSE $P2P_PORT</p>
        <p>ARG IP_ANY=0.0.0.0</p>
        <p># Home folder
          <br />ARG HOME_DIR=/home/sidechain</p>
        <p># Create &apos;sidechain&apos; folder
          <br />RUN mkdir $HOME_DIR
          <br />WORKDIR $HOME_DIR</p>
        <p>ARG BRANCH=master</p>
        <p>RUN apt-get update &amp;&amp; \
          <br />apt-get install -y libncurses5-dev &amp;&amp; \
          <br />apt-get install -y build-essential &amp;&amp; \
          <br />apt-get install -y cmake &amp;&amp; \
          <br />apt-get install -y curl &amp;&amp; \
          <br />apt-get install -y dh-autoreconf &amp;&amp; \
          <br />apt-get install -y expect &amp;&amp; \
          <br />apt-get install -y git &amp;&amp; \
          <br />apt-get install -y libssl-dev &amp;&amp; \
          <br />apt-get install -y doxygen &amp;&amp; \
          <br />apt-get install -y libreadline6 &amp;&amp; \
          <br />apt-get install -y libreadline6-dev &amp;&amp; \
          <br />apt-get install -y libboost-all-dev &amp;&amp; \
          <br />apt-get install -y libzmq3-dev</p>
        <p>RUN git clone -b $BRANCH <a href="https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/sidechain.git">https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/sidechain.git</a>
        </p>
        <p>WORKDIR $HOME_DIR/sidechain</p>
        <p>RUN git submodule update --init --recursive</p>
        <p>RUN cmake -DCMAKE_BUILD_TYPE=Release -DECC_IMPL=secp256k1 .</p>
        <p>RUN make -j 2</p>
        <p>RUN mkdir -p genesis &amp;&amp; \
          <br />programs/witness_node/witness_node --create-genesis-json genesis/my-genesis.json
          &amp;&amp; \
          <br />sh genesis.sh</p>
        <p>#change the lines in a file
          <br />RUN \
          <br />sed -e &apos;s/# p2p-endpoint =/p2p-endpoint = &apos;$IP_ANY&apos;:&apos;$P2P_PORT/
          ./data/my-blockprod/config.ini &gt; thing.txt &amp;&amp; \
          <br />sed -e &apos;s/# rpc-endpoint =/rpc-endpoint = &apos;$IP_ANY&apos;:&apos;$SIDE_PORT/
          thing.txt &gt; thing2.txt &amp;&amp; \
          <br />sed -e &apos;s/# genesis-json =/genesis-json = genesis\/my-genesis.json/&apos;
          thing2.txt &gt; thing.txt &amp;&amp; \
          <br />sed -e &apos;s/# private-key = /private-key = [&quot;PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV&quot;,&quot;5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3&quot;]/&apos;
          thing.txt &gt; thing2.txt &amp;&amp; \
          <br />sed -e &apos;s/# witness-ids =/witness-ids = [&quot;1.6.1&quot;, &quot;1.6.2&quot;,
          &quot;1.6.3&quot;, &quot;1.6.4&quot;, &quot;1.6.5&quot;, &quot;1.6.6&quot;,
          &quot;1.6.7&quot;, &quot;1.6.8&quot;, &quot;1.6.9&quot;, &quot;1.6.10&quot;,
          &quot;1.6.11&quot;]/&apos; thing2.txt &gt; thing.txt &amp;&amp; \
          <br />mv thing.txt data/my-blockprod/config.ini</p>
        <p>RUN chmod +x wallet_node.sh
          <br /># run the witness as service and curl chain_id and run cli_wallet
          <br />ENTRYPOINT [&quot;/bin/bash&quot;, &quot;wallet_node.sh&quot;]</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>  
**wallet\_node.sh**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>## run script wait 5 seconds and exit to avoid the running</p>
        <p>## sleep in bash for loop ##
          <br />nohup programs/witness_node/witness_node --data-dir data/my-blockprod
          --enable-stale-production &amp;&gt; /dev/null &amp;
          <br />sleep 5s
          <br />expect config_wallet.sh $(curl --data &apos;{&quot;jsonrpc&quot;: &quot;2.0&quot;,
          &quot;method&quot;: &quot;get_chain_id&quot;, &quot;id&quot;: 1}&apos;
          <a
          href="http://127.0.0.1:8090/rpc">http://127.0.0.1:8090/rpc</a>| grep -o &apos;\:\&quot;[0-9A-z].*\&quot;&apos;
            | grep -o &apos;[0-9A-z]*&apos;) &quot;<a href="exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md">ws://127.0.0.1:8090</a>&quot;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**genesis.sh**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>## run script wait 5 seconds and exit to avoid the running</p>
        <p>## sleep in bash for loop ##
          <br />programs/witness_node/witness_node --data-dir data/my-blockprod --genesis-json
          genesis/my-genesis.json &amp;&gt; /dev/null &amp;
          <br />sleep 5s
          <br />exit</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**config\_wallet.sh**

| \#!/usr/bin/expect -f \#ignore errors \#set +e \#set +o pipefail set CHAIN\_ID \[lindex $argv 0\]; set CHAIN\_URL\_WS \[lindex $argv 1\]; \# configure the wallet spawn programs/cli\_wallet/cli\_wallet --wallet-file my-wallet.json --chain-id $CHAIN\_ID --server-rpc-endpoint $CHAIN\_URL\_WS expect -exact "new &gt;&gt;&gt; " send -- "set\_password supersecret\r" expect -exact "locked &gt;&gt;&gt; " send -- "unlock supersecret\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key nathan \"5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3\"\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_balance nathan \\[\"5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3\"\\] true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account nathan true\r" expect -exact "unlocked &gt;&gt;&gt; " \#send -- "list\_my\_accounts\r" \#expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com0 PPY7GEP546p7CsMAPVQFSRGrTU11nYrCfcwyAs3r2YXHDfXBikUeM PPY7GEP546p7CsMAPVQFSRGrTU11nYrCfcwyAs3r2YXHDfXBikUeM nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com0 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com0 5Jp4kKVPFu1aB25Sp23cDqREe3SnheoupT6Zb9Us8ztBfMFWM6L\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com1 PPY7dzmuNGKJ5SAAdxkg6aZH7YFkuJM4Qb12WZCKwKMsPjE5VMSUf PPY7dzmuNGKJ5SAAdxkg6aZH7YFkuJM4Qb12WZCKwKMsPjE5VMSUf nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com1 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com1 5KAdgJdAesG5kxMWKjMJBBNZBwwAiegonzCG4TZmdjzA3Stsi6n\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com2 PPY8TXstANsSNN9rMiUzbcwfFa5bKwKEpAbUprwePPoXyH7wTuisp PPY8TXstANsSNN9rMiUzbcwfFa5bKwKEpAbUprwePPoXyH7wTuisp nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com2 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com2 5J2jGrREGb6j72AStZ7Wj7KTXkPe5k7NdLXv3ejBioCEnC6YyFs\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com3 PPY7bQQGWZuXBFU2e75Cg2jh57DDsfi5MrzkY4s5qHNUv7HVWvFPw PPY7bQQGWZuXBFU2e75Cg2jh57DDsfi5MrzkY4s5qHNUv7HVWvFPw nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com3 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com3 5K6GTG8M8Cnmw9BAr4eNJCeEfAgzQgTGRQhFDq6AKFa5b9hoXm8\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com4 PPY8ekyFfwx6gZDKNRPceHHHR2sgTLMTDgL9xSb5Jbv9L9eTNWhfn PPY8ekyFfwx6gZDKNRPceHHHR2sgTLMTDgL9xSb5Jbv9L9eTNWhfn nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com4 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com4 5KGt6AdP7r3HdELrU1CcvdgCWb4JpR2suDuVFcSc78PmPg3U761\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com5 PPY6TzHLATpemLxWdHLsAFjw2LLotaT6JtmvPZBfh9cat4CMMoSyr PPY6TzHLATpemLxWdHLsAFjw2LLotaT6JtmvPZBfh9cat4CMMoSyr nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com5 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com5 5JKENoXacAQaoZicRb5n2CsSR2TRmX1Qs64AeVCPZnmi9Jc8Q2N\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "register\_account com6 PPY63SWcduLXBGt94YwYcTrJH6cDghNYzNye5ha9fDJYueZh9mjyR PPY63SWcduLXBGt94YwYcTrJH6cDghNYzNye5ha9fDJYueZh9mjyR nathan nathan 0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "transfer nathan com6 100000 PPY \"here is the cash\" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "import\_key com6 5JLiAUKjuYFg91mW3U25FZDKzqih1EGnW4Ssymbv6MaaYLawu9H\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com0 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com1 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com2 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com3 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com4 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com5 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "upgrade\_account com6 true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com0 \"[http://www.com0\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com1 \"[http://www.com1\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com2 \"[http://www.com2\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com3 \"[http://www.com3\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com4 \"[http://www.com4\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com5 \"[http://www.com5\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "create\_committee\_member com6 \"[http://www.com6\](exeblock-knowledge-base-how-to-create-sidechain-node-docker-container.md)" true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com0 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com1 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com2 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com3 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com4 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com5 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "vote\_for\_committee\_member nathan com6 true true\r" expect -exact "unlocked &gt;&gt;&gt; " send -- "lock\r" \#expect -exact "locked &gt;&gt;&gt; " exit |
| :--- |


