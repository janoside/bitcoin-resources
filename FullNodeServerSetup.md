## Bitcoin Full Node Server Setup

### OS: Ubuntu 18.04
### Bitcoin Version: 0.18.1

    apt update
    apt upgrade


Attach additional data volumes as needed. For scaleway see [here](https://www.scaleway.com/docs/attach-and-detach-a-volume-to-an-existing-server/#-Step-3--Format-the-additional-volume)

    mkfs -t ext4 /dev/nbd1
    mkdir -p /mnt/data
    mount /dev/nbd1 /mnt/data

Install tor proxy - [ref](https://itrendbuzz.com/install-tor-proxy-on-ubuntu/)

    echo deb https://deb.torproject.org/torproject.org bionic main > /etc/apt/sources.list
    echo deb-src https://deb.torproject.org/torproject.org bionic main >> /etc/apt/sources.list.d/tor.list
    
    curl https://deb.torproject.org/torproject.org/A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89.asc | gpg --import
    
    gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -
    
    sudo apt update
    sudo apt install tor deb.torproject.org-keyring
    
    which tor
    # /usr/sbin/tor
    
    tor --version
    # Tor version 0.4.1.5.
    
    /etc/init.d/tor start
    # Tor proxy starts on 9050


Open `/etc/fstab` and add the line `/dev/nbd1 /mnt/data auto  defaults,nofail,errors=remount-ro 0 2`

    adduser bitcoin
    cd /home/bitcoin
    wget https://bitcoin.org/bin/bitcoin-core-0.18.1/bitcoin-0.18.1-x86_64-linux-gnu.tar.gz
    tar xzfv bitcoin-0.18.1-x86_64-linux-gnu.tar.gz
    rm ./*.gz
    chown -R bitcoin:bitcoin /home/bitcoin/bitcoin-0.18.1/
    wget https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/init/bitcoind.service
    mv bitcoind.service /etc/systemd/system/bitcoind.service
    mkdir /etc/bitcoin
    wget https://raw.githubusercontent.com/janoside/bitcoin-resources/master/files/bitcoin.conf
    mv bitcoin.conf /etc/bitcoin/

Edit config as needed

    vim /etc/bitcoin/bitcoin.conf

Permissions

    chown -R bitcoin:bitcoin /etc/bitcoin
    ln -s /home/bitcoin/bitcoin-0.18.1/bin/bitcoind /usr/bin/
    mkdir /mnt/data/bitcoin
    chown -R bitcoin:bitcoin /mnt/data/bitcoin

Start up

    systemctl daemon-reload
    systemctl enable bitcoind.service
    systemctl start bitcoind.service
