## Bitcoin Full Node Server Setup

### OS: Ubuntu 16.04
### Bitcoin Version: 0.16.3

    apt update
    apt upgrade


Attach additional data volumes as needed. For scaleway see [here](https://www.scaleway.com/docs/attach-and-detach-a-volume-to-an-existing-server/#-Step-3--Format-the-additional-volume)

    mkfs -t ext4 /dev/nbd1
    mkdir -p /mnt/data
    mount /dev/nbd1 /mnt/data

Open `/etc/fstab` and add the line `/dev/nbd1 /mnt/data auto  defaults,nofail,errors=remount-ro 0 2`

    adduser bitcoin
    cd /home/bitcoin
    wget https://bitcoin.org/bin/bitcoin-core-0.16.3/bitcoin-0.16.3-x86_64-linux-gnu.tar.gz
    tar xzfv bitcoin-0.16.3-x86_64-linux-gnu.tar.gz
    rm ./*.gz
    chown -R bitcoin:bitcoin /home/bitcoin/bitcoin-0.16.3/
    wget https://raw.githubusercontent.com/bitcoin/bitcoin/master/contrib/init/bitcoind.service
    mv bitcoind.service /etc/systemd/system/bitcoind.service
    mkdir /etc/bitcoin
    wget https://raw.githubusercontent.com/janoside/bitcoin-resources/master/files/bitcoin.conf
    mv bitcoin.conf /etc/bitcoin/


Edit config as needed

    vim /etc/bitcoin/bitcoin.conf



    chown -R bitcoin:bitcoin /etc/bitcoin
    ln -s /home/bitcoin/bitcoin-0.16.3/bin/bitcoind /usr/bin/
    mkdir /mnt/data/bitcoin
    chown -R bitcoin:bitcoin /mnt/data/bitcoin
    systemctl daemon-reload
    systemctl enable bitcoind.service
    systemctl start bitcoind.service
