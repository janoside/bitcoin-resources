## Bitcoin Full Node Server Setup

### OS: Ubuntu 16.04
### Bitcoin Version: 0.15.1

    apt update
    apt upgrade


* Attach additional data volumes as needed. For scaleway see [here](https://www.scaleway.com/docs/attach-and-detach-a-volume-to-an-existing-server/#-Step-3--Format-the-additional-volume)



    mkfs -t ext4 /dev/nbd1
    mkdir -p /mnt/data
    mount /dev/nbd1 /mnt/data


* Open `/etc/fstab` and add the line `/dev/nbd1 /mnt/data auto  defaults,nofail,errors=remount-ro 0 2`


    wget https://bitcoin.org/bin/bitcoin-core-0.15.1/bitcoin-0.15.1-x86_64-linux-gnu.tar.gz
    tar xzfv bitcoin-0.15.1-x86_64-linux-gnu.tar.gz
    rm ./*.gz
    adduser bitcoin
    mv /root/bitcoin-0.15.1/ /home/bitcoin/
    chown -R bitcoin:bitcoin bitcoin-0.15.1/


* Copy content from [https://github.com/bitcoin/bitcoin/blob/master/contrib/init/bitcoind.service](https://github.com/bitcoin/bitcoin/blob/master/contrib/init/bitcoind.service) into /etc/systemd/system/bitcoin.service


    mkdir /etc/bitcoin

* Copy content from [./files/bitcoin.conf](./files/bitcoin.conf) into /etc/bitcoin/bitcoin.conf


    chown -R bitcoin:bitcoin /etc/bitcoin
    ln -s /home/bitcoin/bitcoin-0.15.1/bin/bitcoind /usr/bin/
    systemctl daemon-reload
