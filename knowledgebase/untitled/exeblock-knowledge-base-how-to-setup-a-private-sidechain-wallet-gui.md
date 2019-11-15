# EXEBlock Knowledge Base : How to setup a Private Sidechain wallet Gui

## Setting up private testnet <a id="HowtosetupaPrivateSidechainwalletGui-Settingupprivatetestnet"></a>

**Important Note: This is for running the wallet locally, different than running it within a docker container.**

## Step-by-step guide <a id="HowtosetupaPrivateSidechainwalletGui-Step-by-stepguide"></a>

For Ubuntu 14.04 LTS and up users, see this link first: [https://github.com/cryptonomex/graphene/wiki/build-ubuntu](https://github.com/cryptonomex/graphene/wiki/build-ubuntu)

and then proceed:

1. **Getting code and compiling:** _update and add libraries needed \(if using a Mac install these pieces or alternatives using brew install\)_ apt-get update \ && apt-get install -y curl \ && apt-get install -y git \ && apt-get install -y bash \ && apt-get install -y nodejs \ && apt-get install -y npm \ && apt-get install -y g++ \ && apt-get install -y python \ && apt-get install -y make \ && apt-get install -y libpng-dev \ && apt-get install -y autoconf \ && apt-get install -y automake \ && apt-get install -y libtool \ && apt-get install -y nasm  _install npm and n_ npm install -g n n 6.7.0  _now get the sidechain-wallet repo, and peerplaysjs-lib repo_ git clone [git@bitbucket.org](mailto:git@bitbucket.org):exeblock/sidechain-wallet-gui.git git clone git@bitbucket.org:exeblock/peerplaysjs-lib cd peerplaysjs-lib npm install  cd .. cd sidechain-wallet-gui/dl npm install  cd .. cd web npm install npm rebuild node-sass 
2. **Run in the sidechain-wallet-gui/web:**  
   npm start  
  
  


   _\*\*if npm fail from a port already used, kill the process that is using it and run npm start again_

