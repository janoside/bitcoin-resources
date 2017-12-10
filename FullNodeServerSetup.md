## Bitcoin Full Node Server Setup

### OS: Ubuntu 16.04
### Bitcoin Version: 0.15.1

    apt update
    apt upgrade
    wget https://bitcoin.org/bin/bitcoin-core-0.15.1/bitcoin-0.15.1-x86_64-linux-gnu.tar.gz
    tar xzfv bitcoin-0.15.1-x86_64-linux-gnu.tar.gz
    rm ./*.gz
    adduser bitcoin
    mv /root/bitcoin-0.15.1/ /home/bitcoin/
    chown -R bitcoin:bitcoin bitcoin-0.15.1/

* Copy content from [https://github.com/bitcoin/bitcoin/blob/master/contrib/init/bitcoind.service](https://github.com/bitcoin/bitcoin/blob/master/contrib/init/bitcoind.service) into /etc/systemd/system/bitcoin.service
