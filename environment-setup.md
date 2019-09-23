# Bitcoin Environment Setup

## Prerequisites

* Ubuntu 18.04 Server

## Goals

1. Compile `Bitcoin Core` from source and start it
2. Install and configured `Electrum Personal Server` (EPS)
3. Configure "Private" Electrum wallet to communicate **only** with EPS.


-----

### Goal 1: Bitcoin Core

1. Before compiling Bitcoin Core, you'll need to install Berkeley DB 4.8:

        wget http://download.oracle.com/berkeley-db/db-4.8.30.zip
        unzip db-4.8.30.zip
        cd db-4.8.30
        cd build_unix/
        ../dist/configure --prefix=/usr/local --enable-cxx
        make
        make install
    
2. [Compile Bitcoin Core](https://bitzuma.com/posts/compile-bitcoin-core-from-source-on-ubuntu/)



-----


### Goal 2: Electrum Personal Server (EPS)

This software provides a blockchain-data API to the Electrum wallet (hereafter "Electrum") software that we'll install later. By default Electrum will connect to any number of public "Electrum Servers" which feed Electrum data but can, in doing so, spy on you and your data. EPS provides a compatible API that will only manage and serve *your* data.

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


-----

### Goal 3: Configure "Private" Electrum (macOS)

**Prerequisite**: Install Electrum for macOS.

Now, to avoid privacy leaks associated with connecting to public Electrum servers, we create an AppleScript Application to launch Electrum.app in single-server "private mode". Launching Electrum via this AppleScript Application will configure it at launch to only connect to your EPS instance.

1. Open Apple's "Script Editor"
2. New Document, type=Application, name=ElectrumPrivate
3. Paste text (testnet optional):

       do shell script "open -a /Applications/Electrum.app --args --oneserver --server 192.168.1.6:50002:s --testnet"

4. Save
5. From now on, launch "ElectrumPrivate" app instead of "Electrum"


-----

### Goal 4: Configure ElectrumX Server

1. Install

        pip3 install plyvel
        git clone https://github.com/kyuupichan/electrumx.git
        cd electrumx
        pip3 install .
        cp contrib/systemd/electrumx.service /etc/systemd/system/

2. Edit `electrumx.service`

        ...
        ExecStart=/path/to/electrumx/git/electrumx_server
        User=yourusername
        ...
        
3. Create config file `/etc/electrumx.conf` with the following contents:

        COIN=BitcoinSegwit
        DB_DIRECTORY=/path/to/data-dir/
        DAEMON_URL=bitcoin-rpc-username:bitcoin-rpc-password@127.0.0.1
        
4. Start and monitor

        systemctl daemon-reload
        systemctl enable bitcoind.service   # start at boot
        service electrumx start
        journalctl -u electrumx -f
