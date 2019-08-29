# Bitcoin Environment Setup

### Prerequisites

* Ubuntu 18.04 Server

### Bitcoin Core Setup

Before compiling Bitcoin Core, you'll need to install Berkeley DB 4.8:

    wget http://download.oracle.com/berkeley-db/db-4.8.30.zip
    unzip db-4.8.30.zip
    cd db-4.8.30
    cd build_unix/
    ../dist/configure --prefix=/usr/local --enable-cxx
    make
    make install


### Electrum Personal Server

Ref: https://stadicus.github.io/RaspiBolt/raspibolt_64_electrum.html

    sudo apt install -y python3 python3-pip
    sudo pip3 install setuptools
    
    git clone git@github.com:chris-belcher/electrum-personal-server.git
    cd electrum-personal-server
    cp config.ini_sample config.ini

Add your wallet master public keys or watch-only addresses to the `[master-public-keys]` and `[watch-only-addresses]` sections. Master public keys for an Electrum wallet can be found in the Electrum client menu `Wallet -> Information`. Multisig (M-of-N) wallets should be entered in the form `M xpub1... xpub2... ... xpubN...`.

    wallet1 = xpub...
    wallet2 = xpub...
    wallet3 = 2 xpub1... xpub2...

Install (`wheel` prerequisite package installed first):

    pip3 install wheel
    pip3 install --user .


### Electrum (macOS)

Create AppleScript Application to launch Electrum.app in single-server "private mode" (only connects to your Electrum Personal Server instance):

* Open Apple's "Script Editor"
* New Document, type=Application, name=ElectrumPrivate
* Paste text (testnet optional):

      do shell script "open -a /Applications/Electrum.app --args --oneserver --server 192.168.1.6:50002:s --testnet"

* Save
* Launch "ElectrumPrivate" app
