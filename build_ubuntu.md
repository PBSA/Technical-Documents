# BUILD\_UBUNTU

Note: this documents is a copy of "Build" section of [README.md](https://github.com/bitshares/bitshares-core/blob/develop/README.md), it may be stale, please check README.md for latest info.

## Ubuntu 16.04 LTS \(64-bit\) Build and Install Instructions

The following dependencies were necessary for a clean install on Ubuntu 16.04 LTS \(64-bit\):

```text
sudo apt-get update
sudo apt-get install autoconf cmake make automake libtool git libboost-all-dev libssl-dev g++ libcurl4-openssl-dev
```

### Build BitShares Core

```text
git clone https://github.com/bitshares/bitshares-core.git
cd bitshares-core
git submodule update --init --recursive
cmake -DCMAKE_BUILD_TYPE=Release .
make 
```

### Build Support Boost Version

NOTE: BitShares requires a Boost version in the range \[1.58 - 1.69\]. Newer versions may work, but have not been tested. If your system came pre-installed with a version of Boost that you do not wish to use, you may manually build your preferred version and use it with BitShares by specifying it on the CMake command line. Example:

```text
cmake -DBOOST_ROOT=/path/to/boost -DCMAKE_BUILD_TYPE=Release .
make
```

