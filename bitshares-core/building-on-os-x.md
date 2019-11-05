# Building-on-OS-X

1. Install XCode and its command line tools by following the instructions here: [https://guide.macports.org/\#installing.xcode](https://guide.macports.org/#installing.xcode). 

   In OS X 10.11 \(El Capitan\) and newer, you will be prompted to install developer tools when running a devloper command in the terminal. This step may not be needed.

2. Install Homebrew by following the instructions here: [http://brew.sh/](http://brew.sh/)
3. Initialize Homebrew:

   ```text
   brew doctor
   brew update
   ```

4. Install dependencies:

   ```text
   brew install boost cmake git openssl autoconf automake libtool 
   brew link --force openssl
   ```

5. _Optional._ To support importing Bitcoin wallet files:

   ```text
   brew install berkeley-db
   ```

6. _Optional._ To use TCMalloc in LevelDB:

   ```text
   brew install google-perftools
   ```

7. Clone the Bitshares repository:

   ```text
   git clone https://github.com/bitshares/bitshares-core.git
   cd bitshares-core
   ```

8. Build BitShares:

   ```text
   git submodule update --init --recursive
   cmake .
   make
   ```

   Notes: As mentioned elsewhere, Bitshares depends on the third-party libraries "Boost" and "OpenSSL". These libraries need to be in certain version ranges. At the moment, Boost needs to be between 1.58 and 1.69. OpenSSL needs to be in the 1.0.x or 1.1.x range.

Boost: You can check which version\(s\) of boost you have by asking brew:

```text
   brew search boost
```

To install another version of Boost \(such as 1.60\):

```text
   brew install boost@1.60
```

OpenSSL: You may have an older version of OpenSSL than is required. If so, have brew get the latest:

```text
   brew upgrade openssl
```

Compiling with these new versions: We must now tell cmake where these libraries are. Instead of the "cmake ." mentioned above, we use:

```text
   cmake -DBOOST_ROOT=/usr/local/opt/boost@1.60 -DOPENSSL_ROOT_DIR=/usr/local/opt/openssl .
```

and then proceed with the normal

```text
   make
```

