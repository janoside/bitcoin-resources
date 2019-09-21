# Bitcoin Environment Setup

### Prerequisites

* Ubuntu 18.04 Server

### Bitcoin Core Setup

1. Before compiling Bitcoin Core, you'll need to install Berkeley DB 4.8:

        wget http://download.oracle.com/berkeley-db/db-4.8.30.zip
        unzip db-4.8.30.zip
        cd db-4.8.30
        cd build_unix/
        ../dist/configure --prefix=/usr/local --enable-cxx
        make
        make install
    
2. [Compile Bitcoin Core](https://bitzuma.com/posts/compile-bitcoin-core-from-source-on-ubuntu/)


### Electrum Personal Server

1. Install [source](https://stadicus.github.io/RaspiBolt/raspibolt_64_electrum.html)

        sudo apt install -y python3 python3-pip
        sudo pip3 install setuptools
    
        git clone git@github.com:chris-belcher/electrum-personal-server.git
        cd electrum-personal-server
        cp config.ini_sample config.ini

2. Add your wallet master public keys or watch-only addresses to the `[master-public-keys]` and `[watch-only-addresses]` sections. Master public keys for an Electrum wallet can be found in the Electrum client menu `Wallet -> Information`. Multisig (M-of-N) wallets should be entered in the form `M xpub1... xpub2... ... xpubN...`.

        wallet1 = xpub...
        wallet2 = xpub...
        wallet3 = 2 xpub1... xpub2...

3. Install (`wheel` prerequisite package installed first):

        pip3 install wheel
        pip3 install --user .

4. Run

        nohup electrum-personal-server /path/to/electrum-personal-server/config.ini > eps.log &


### Electrum (macOS)

Goal: Create AppleScript Application to launch Electrum.app in single-server "private mode" (only connects to your Electrum Personal Server instance):

1. Open Apple's "Script Editor"
2. New Document, type=Application, name=ElectrumPrivate
3. Paste text (testnet optional):

       do shell script "open -a /Applications/Electrum.app --args --oneserver --server 192.168.1.6:50002:s --testnet"

4. Save
5. Launch "ElectrumPrivate" app
