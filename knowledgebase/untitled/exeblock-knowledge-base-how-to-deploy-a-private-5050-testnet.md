# EXEBlock Knowledge Base : How to deploy a private 5050 testnet

\*\*\*Notes:

build.sh / build\_run.sh scripts use a built-in functionality to read an internal IP address of the target machine. For Mac users, to support such functionality, it is required to install iproute package: _brew install iproute2mac_

1. Clone the testnet-docker repository  _git clone_ [_https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock_](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock)[_/testnet-docker.git_](https://alexmorgulis@bitbucket.org/exeblock/testnet-docker.git)  __
2. Create a script or just run these commands \(the order matters, so run the peerplays-chain first, then the tapin second and the 5050dapp last\):  


   _echo "removing 5050dapp container"_  
   _sudo docker ps -a \| awk '{ print $1,$2 }' \| grep 5050dapp \| awk '{print $1 }' \| xargs -I {} sudo docker rm -f {}_

   _echo "removing peerplays-chain container"_  
   _sudo docker ps -a \| awk '{ print $1,$2 }' \| grep peerplays-chain \| awk '{print $1 }' \| xargs -I {} sudo docker rm -f {}  
  
   echo "removing tapin container"_  
   _sudo docker ps -a \| awk '{ print $1,$2 }' \| grep tapin \| awk '{print $1 }' \| xargs -I {} sudo docker rm -f {}_

   _echo "cleaning up environment"_  
   _sudo docker container prune -f_  
   _sudo docker image prune -f_  
   echo "build and run peerplays-chain container"  
   _cd peerplays-chain-docker_  
   _chmod +x build.sh_  
   _chmod +x build\_run.sh_  
   _./build\_run.sh  
   cd .._  
  
   echo "build and run tapin container"  
   _cd tapin-docker_  
   _chmod +x build.sh_  
   _chmod +x build\_run.sh_  
   _./build\_run.sh  
   cd .._  
  
   echo "build and run 5050dapp container"  
   _cd 5050dapp-docker_  
   _chmod +x build.sh_  
   _chmod +x build\_run.sh_  
   _./build\_run.sh_  
  

3.  Open 5050 web site and create a new account. Make sure you can sign up and login into the application.   

