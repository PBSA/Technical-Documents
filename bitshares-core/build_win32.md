# BUILD\_WIN32

This document will help you build Bitshares Core from its source code, assuming only that you have a fresh installation of Windows.

Note that compiling BitShares Core is not required to use the software. Pre-built binaries are available [here](https://github.com/bitshares/bitshares-core/releases).

#### Prerequisites

* 64 bit windows operating system. Windows 10 was used for this document.

## Visual Studio

Visual studio 2019 is used by some in the core team. Visual Studio 2017 and 2015 Update 1 are known to have worked in the past. Assume that anything older is unsupported. The code contains C++14 features, so older compilers that do not support the features used will not work.

## Git

Download Git for Windows [here](https://git-scm.com/download/win).

Installation of git is straightforward. Using the installation options that adds to your PATH environment variable is recommended.

## CMake

Download the latest cmake and install it. Version 3.14.2 was the latest version when this document was prepared. Find it available for download [here](https://cmake.org/download).

Install it, and allow it to add to your PATH environment variable.

## Perl

Perl is used to build OpenSSL. ActiveState Perl is the recommended Perl engine. Version 5.26.3 was used for prepare this document. ActivePerl can be found [here](https://www.activestate.com/ActivePerl).

Install it. As usual, adding this to the PATH will make your life easier.

## NASM

NASM is also used to build some pieces of OpenSSL. A direct link to the version used to prepare this document is [here](https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/win64/nasm-2.14.02-installer-x64.exe).

The installer seems to not add to the PATH environment variable. You should do this manually.

## OpenSSL

OpenSSL, Boost, and libcurl are libraries compiled into the eventual BitShares-Core application. The instructions from here on can be modified to fit your situation. The `VS2015 x64 Native Tools command prompt` was used as `Administrator` to run these commands.

Advice: To make life easier, you may wish to make a folder called "Development", where your compilation work can be kept. Within "Development", make a folder called "cpp" where you do everything c++ related. From there, you can type:

`git clone https://github.com/openssl/openssl`

This will pull the latest source code for OpenSSL to your machine. Since I do not care to put experimental versions of libraries into BitShares, I then get a more stable version and compile by typing the following commands:

```text
cd openssl
git checkout tags/OpenSSL_1_1_1b
perl ./Configure VC-WIN64A no-shared
```

This may give an error about not having nmake.exe or dmake.exe in your PATH. If so, do not worry. If you can run nmake from this directory, you will be fine. Now type the following commands

```text
nmake
nmake install
```

By default, this will put OpenSSL in your C:\Program Files\OpenSSL directory.

## Boost

Return to your C:\Development\cpp directory \("cd .." should get you there\), then clone and compile the boost library by typing the following commands:

```text
git clone https://github.com/boostorg/boost
cd boost
git checkout tags/boost-1.69.0
git submodule update --init --recursive
bootstrap.bat
b2 --prefix=c:\Development\cpp\boost169 --toolset=msvc variant=release threading=multi address-model=64 install
```

Note that if you have more than one msvc compiler installed, you will need to modify the `toolset` parameter.

| compiler | toolset parameter |
| :--- | :--- |
| Visual Studio 2015 | msvc-14.0 |
| Visual Studio 2017 | msvc-14.1 |
| Visual Studio 2019 | msvc-14.2 |

## libcurl

Return to your C:\Development\cpp directory and type:

```text
git clone https://github.com/curl/curl
cd curl
git checkout tags/curl-7_64_1
biuldconf.bat
mkdir curl-build
cd curl-build
cmake -G "Visual Studio 16 2019" -A x64 -DCMAKE_USE_OPENSSL=ON -DCURL_DISABLE_FTP=ON -DCURL_DISABLE_LDAP=ON -DCURL_DISABLE_TELNET=ON -DCURL_DISABLE_DICT=ON -DCURL_DISABLE_FILE=ON -DCURL_DISABLE_TFTP=ON -DCURL_DISABLE_LDAPS=ON -DCURL_DISABLE_RTSP=ON -DCURL_DISABLE_POP3=ON -DCURL_DISABLE_IMAP=ON -DCURL_DISABLE_SMTP=ON -DCURL_DISABLE_GOPHER=ON -DCURL_STATICLIB=ON ..\
cmake --build . --target install --config Release
```

### Bitshares Core

Return once again to your C:\Development\cpp directory and type:

```text
git clone https://github.com/bitshares/bitshares-core
cd bitshares-core
git submodule update --init --recursive
cmake -G "Visual Studio 16 2019" -A x64 -DBOOST_ROOT=c:\Development\cpp\boost169 -DCURL_STATICLIB=ON -DCURL_LIBRARY="C:\Program Files\CURL\lib\libcurl_imp.lib" -DCURL_INCLUDE_DIR="C:\Program Files\CURL\include" -DOPENSSL_CONF_SOURCE="C:\Program Files\Common Files\SSL\openssl.cnf"
cmake --build . --target install --config Release
```

Upon success, you will have the applications compiled. You can find the application "witness\_node" at `c:\Development\cpp\bitshares-core\programs\witness_node\witness_node.exe`. The application "cli\_wallet" can be found at `c:\Development\cpp\bitshares-core\programs\cli_wallet\cli_wallet.exe`.

