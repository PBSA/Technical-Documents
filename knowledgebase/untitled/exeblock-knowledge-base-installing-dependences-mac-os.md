# EXEBlock Knowledge Base : Installing dependences \(Mac OS\)

Running Peerplays locally equivalent to BitShares. So we are going to install all dependences to run the BTS Tools and BTS GUI.

## Step-by-step guide <a id="Installingdependences(MacOS)-Step-by-stepguide"></a>

1. Open Terminal window
2. Install Homebrew $ /usr/bin/ruby -e "$\(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install\)"
3. $ brew install cmake
4. $ brew install libyaml
5. On OSX, you should install dependencies with brew instead of building your own, as the current libs in brew are recent enough to allow to compile the BitShares client. You will also need to force the install of `readline` system-wide and override OSX’s native version, as it is antiquated.  
   $ brew install git cmake boost readline openssl autoconf automake libtool berkeley-db  
   $ brew link --force readline

   ```text

   ```

   System Integrity Protection \(rootless\) is enable in Mac OS from version 10.11 onward. It will lock down the system level directories. Accordingly, rootless may cause some apps, utilities, and scripts to not function at all, even with sudo privilege, [root user enabled](http://osxdaily.com/2015/02/19/enable-disable-root-command-line-mac/), or admin access. [See related article](http://osxdaily.com/2015/10/05/disable-rootless-system-integrity-protection-mac-os-x/).

## Related articles <a id="Installingdependences(MacOS)-Relatedarticles"></a>

[http://bts-tools.readthedocs.io/en/latest/install.html\#mac-osx](http://bts-tools.readthedocs.io/en/latest/install.html#mac-osx)

[https://brew.sh](https://brew.sh)

[https://stackoverflow.com/questions/32185079/installing-cmake-with-home-brew](https://stackoverflow.com/questions/32185079/installing-cmake-with-home-brew)

[http://osxdaily.com/2015/10/05/disable-rootless-system-integrity-protection-mac-os-x/](http://osxdaily.com/2015/10/05/disable-rootless-system-integrity-protection-mac-os-x/)

*  Page: [Installing dependences \(Mac OS\)](/wiki/spaces/EKB/pages/197460113)

