# EXEBlock Knowledge Base : How to Setup EXEchain

## Instructions for building the project as of the Oct 17, 2018 release <a id="HowtoSetupEXEchain-InstructionsforbuildingtheprojectasoftheOct17,2018release"></a>

  
**Install Commands**

```text
sudo apt-get update
sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git ntp libcurl4-openssl-dev g++ libleveldb-dev
mkdir exechain
cd exechain
git clone https://bitbucket.org/exeblock/exeblock.git .
git submodule update --init --recursive
cd eos
./build.sh ubuntu
cd ..
cmake .
cmake --build .
```

## The Following Instructions refer releases pre-dating Oct 17, 2018 and are being kept for future reference only <a id="HowtoSetupEXEchain-TheFollowingInstructionsreferreleasespre-datingOct17,2018andarebeingkeptforfuturereferenceonly"></a>

\# Ubuntu 16.04 LTS Build and Install Instructions

  
**The following dependencies are necessary for a clean install of Ubuntu 16.04 LTS:**

```text
sudo apt-get update
sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git ntp libcurl4-openssl-dev g++ libleveldb-dev
```

\#\# Build Boost 1.65.1

  
**Exeblock build process requires the Boost tarball for Boost 1.65.1**

```text
wget https://dl.bintray.com/boostorg/release/1.65.1/source/boost_1_65_1.tar.gz
tar -xvzf boost_1_65_1.tar.gz
cd boost_1_65_1/
pwd (copy path) 
BOOST_ROOT=$HOME/root/exeblock/boost_1_65_1
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install
```

\#\# Access to Exeblock submodules 

  
**Before downloading and building project you must replace submodules URL's from \`bitbucket.org\` to \`gitlab.pixelplex.by\` in git global config.**

```text
git config --global url."https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/fc.git".insteadOf "https://pixel-plex@bitbucket.org/exeblock/fc.git"
git config --global url."https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/ethereum.git".insteadOf "https://pixel-plex@bitbucket.org/exeblock/ethereum.git"
git config --global url."https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/eos.git".insteadOf "https://pixel-plex@bitbucket.org/exeblock/eos.git"
git config --global url."https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/secp256k1-zkp.git".insteadOf "https://pixel-plex@bitbucket.org/exeblock/secp256k1-zkp.git"
```

\#\# Build Exeblock

```text
git clone https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/exeblock.git
cd exeblock
git submodule update --init --recursive
cd eos
./build.sh ubuntu
cd ../cpp-ethereum/
cmake -DBUILD_SHARED_LIBS=ON .
cmake --build .
cd ../cpp-ethereum/deps/
wget https://github.com/open-source-parsers/jsoncpp/archive/1.7.7.tar.gz
tar -xvzf 1.7.7.tar.gz
mv jsoncpp-1.7.7/ jsoncpp
cd ../..
cmake -DBOOST_ROOT="$BOOST_ROOT" .
make -j 3
```

\#\# Start ExeBlock with Docker

  
**Docker must be installed on your device. After that you have to download exeblock image.**

```text
docker pull exeblock
```

  
**Create shared memory \(volume\).**

```text
docker volume create my-vol
```

  
**And now you may start docker container with node.**

```text
sudo docker run -p 8090-8091:8090-8091 -v exeblock_dir:/app exeblock -dapp
```

