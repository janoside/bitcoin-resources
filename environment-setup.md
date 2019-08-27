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
    
    mkdir electrum-personal-server
    cd electrum-personal-server
    wget https://github.com/chris-belcher/electrum-personal-server/archive/eps-v0.1.6.tar.gz
    wget https://github.com/chris-belcher/electrum-personal-server/releases/download/eps-v0.1.6/eps-v0.1.6.tar.gz.asc
    wget https://raw.githubusercontent.com/chris-belcher/electrum-personal-server/master/docs/pubkeys/belcher.asc
    gpg --import belcher.asc
    gpg --verify eps-v0.1.6.tar.gz.asc
    
Check for:

    > gpg: Good signature from "Chris Belcher <false@email.com>" [unknown]
    > Primary key fingerprint: 0A8B 038F 5E10 CC27 89BF  CFFF EF73 4EA6 77F3 1129

More...

    tar -xvf eps-v0.1.6.tar.gz
    rm *.gz*
    cp electrum-personal-server-eps-v0.1.6/config.cfg_sample config.cfg

Add your wallet master public keys or watch-only addresses to the `[master-public-keys]` and `[watch-only-addresses]` sections. Master public keys for an Electrum wallet can be found in the Electrum client menu `Wallet -> Information`.

Install...

    cd electrum-personal-server-eps-v0.1.6/
    # Install the wheel package first, which is required
    pip3 install wheel
    pip3 install --user .
