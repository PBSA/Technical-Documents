# EXEBlock Knowledge Base : How to create sidechain-wallet-gui docker container

\*\*NOTE: See attached files  
\*\*NOTE: This setup is for the sidechain wallet which works with:  
1\) older peerplays libraries \(peerplaysjs\) and current sidechain blockchain.  
2\) latest peerplays libraries \(peerplays-graphene\) and the baxter blockchain.

INSTRUCTIONS:

1. Copy the Dockerfile content below into a new file called Dockerfile.
2. In the same directory as the Dockerfile, copy the gui-wallet-setup.sh file below.
3. In the same directory, use the following command to build the image:

   ```text
   docker build -t exeblock/sidechain-wallet-gui --no-cache .
   ```

4.  When completed, run the docker:  
   Two parameters can be set for the configuration by using the -e flag: CHAIN\_URL, FAUCET\_URL:

   ```text
   docker run -dit --name sidechain-wallet-gui -p 8082:8082 -e CHAIN_URL="ws://localhost:8090/ws" -e FAUCET_URL="http://localhost:3000/faucet"  -e FAUCET_URLS="http://localhost:3000/faucet" exeblock/sidechain-wallet-gui
   ```

   \*\*NOTE: FAUCET\_URLS will default to FAUCET\_URL if not provided.

5. To debug:

   ```text
   docker exec -it sidechain-wallet-gui /bin/bash
   ```

## For current sidechain blockchain \(using older Peerplays libs\) <a id="Howtocreatesidechain-wallet-guidockercontainer-Forcurrentsidechainblockchain(usingolderPeerplayslibs)"></a>

### Dockerfile <a id="Howtocreatesidechain-wallet-guidockercontainer-Dockerfile"></a>

```text
FROM ubuntu:16.04

# URL to use for connecting to the blockchain
ARG chain_url
ENV CHAIN_URL=$chain_url

# URL to use for connecting to the faucet
ARG faucet_url
ENV FAUCET_URL=$faucet_url

# URLs for connecting to the faucet (in dl/src/common/faucetUrls.json)
ARG faucet_urls
ENV FAUCET_URLS=$faucet_urls

# Home folder
ARG HOME_DIR=/home

# Sidechain Wallet folder

ARG GUI_DIR=$HOME_DIR/sidechain-wallet-gui

# dl folder

ARG DL_DIR=$GUI_DIR/dl

# web folder

ARG WEB_DIR=$GUI_DIR/web

# Update and add necessary libraries

RUN apt-get update \
  && apt-get install -y curl \
  && apt-get install -y git \
  && apt-get install -y npm \
  && apt-get install -y g++ \
  && apt-get install -y python \
  && apt-get install -y make \
  && apt-get install -y libpng-dev \
  && apt-get install -y autoconf \
  && apt-get install -y automake \
  && apt-get install -y libtool \
  && apt-get install -y nasm 

# Use nodejs v6.7.0
RUN npm install -g n
RUN n 6.7.0

# Clone sidechain-wallet-gui into home

WORKDIR $HOME_DIR
RUN git clone -b dockerfile-sidechain-build https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/sidechain-wallet-gui.git

# Clone peerplaysjs-lib into home
WORKDIR $HOME_DIR
RUN git clone https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/peerplaysjs-lib.git

# Build peerplaysjs-lib
WORKDIR $HOME_DIR/peerplaysjs-lib
RUN npm install --unsafe-perm

# Build gui-wallet
WORKDIR $DL_DIR
RUN npm install --unsafe-perm

WORKDIR $WEB_DIR
RUN npm install --unsafe-perm

# Copies wallet gui setup script to this environment

COPY ./wallet-gui-setup.sh /
RUN chmod +x /wallet-gui-setup.sh

# Port
EXPOSE 8082

# Runs script which passes in config variables and starts wallet
CMD [ "/bin/bash", "/wallet-gui-setup.sh"]
```

Config settings for the server can be found under peerplays-wallet-gui/web/conf/webpack.config.js

\`\`NOTE: --unsafe-perm flags are necessary for the libraries to build properly in this environment.

### wallet-gui-setup.sh <a id="Howtocreatesidechain-wallet-guidockercontainer-wallet-gui-setup.sh"></a>

```text
#!/bin/bash

sed -i "36s|.*|BLOCKCHAIN_URL: JSON.stringify(options.blockchain \|\| \"${CHAIN_URL:-ws/localhost:8090/ws}\"),|" /home/sidechain-wallet-gui/web/conf/webpack.config.js
sed -i "37s|.*|FAUCET_URL: JSON.stringify(options.faucet \|\| \"${FAUCET_URL:-http://localhost:3000/faucet}\"),|" /home/sidechain-wallet-gui/web/conf/webpack.config.js
sed -i "2s|.*|\"0\": \"${FAUCET_URLS:-$FAUCET_URL}\"|" /home/sidechain-wallet-gui/dl/src/common/faucetUrls.json

cd /home/sidechain-wallet-gui/web/

npm start
```

\`\`NOTE: If you are trying to host this on a server and get an invalid host header error, add the follow code inside web/server.js for a quick fix:

```text
disableHostCheck: true
```

**This is a temporary solution, and is not recommended for production.**

## For sidechain merge-baxter blockchain \(using new Peerplays libs\) <a id="Howtocreatesidechain-wallet-guidockercontainer-Forsidechainmerge-baxterblockchain(usingnewPeerplayslibs)"></a>

Dockerfile

```text
FROM ubuntu:16.04
 
# URL to use for connecting to the blockchain
ARG chain_url
ENV CHAIN_URL=$chain_url
 
# URL to use for connecting to the faucet
ARG faucet_url
ENV FAUCET_URL=$faucet_url
 
# URLs for connecting to the faucet (in dl/src/common/faucetUrls.json)
ARG faucet_urls
ENV FAUCET_URLS=$faucet_urls

# Home folder
ARG HOME_DIR=/home
 
# Sidechain Wallet folder
ARG GUI_DIR=$HOME_DIR/sidechain-wallet-gui-merge
ENV GUI_DIR=$GUI_DIR
 
# dl folder
ARG DL_DIR=$GUI_DIR/dl
 
# web folder
ARG WEB_DIR=$GUI_DIR/web
 
# Update and add necessary libraries
RUN apt-get update \
  && apt-get install -y curl \
  && apt-get install -y git \
  && apt-get install -y npm \
  && apt-get install -y g++ \
  && apt-get install -y python \
  && apt-get install -y make \
  && apt-get install -y libpng-dev \
  && apt-get install -y autoconf \
  && apt-get install -y automake \
  && apt-get install -y libtool \
  && apt-get install -y nasm 

# Use nodejs v6.7.0
RUN npm install -g n
RUN n 6.7.0

# Clone sidechain-wallet-gui into home
WORKDIR $HOME_DIR
RUN git clone -b sidechain-wallet https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/sidechain-wallet-gui-merge.git
 
# Build gui-wallet
WORKDIR $DL_DIR
RUN npm install --unsafe-perm

WORKDIR $WEB_DIR 
RUN npm install --unsafe-perm
 
# Copies wallet gui setup script to this environment
COPY ./wallet-gui-setup.sh /
RUN chmod +x /wallet-gui-setup.sh

# Port
EXPOSE 8082
 
# Builds the dev server
CMD [ "/bin/bash", "/wallet-gui-setup.sh" ]
```

If encountering "Uncaught Error: Cannot find module './.json'.", make sure that your webpack.config.js is configured properly. Easiest way is to reset it by checking out the latest version from the branch, and re-configuring it. Make sure that you have also specified the faucet under dl/src/common/faucetUrls.json

