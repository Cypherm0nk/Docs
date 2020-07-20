# **Running Liquid (Linux)** 
### **Getting Started with Liquid**

##### This tutorial will instruct complete beginners on how to get started using Liquid (in production) as quickly as possible with the following steps:

1. Install Bitcoin Core
    * Create bitcoin.conf
2. Install Elements
    * Create elements.conf
3. Begin Syncing

### Required:
* 30 GB of disk space
* 8 GB of RAM
* x86_64-linux-gnu

### **1. Install Bitcoin Core**
##### The codeblock below can be copied and pasted directly into your terminal. `wget` is used to download the latest Bitcoin Core release and its SHAsums. `sha256sum` is used to verify the compressed download. `gpg` is used to import the pgp key that signs binary releases and verify the signed, compressed binaries. `tar` is used to extract the binaries. `sudo install` is used to invoke administrator priveleges and then install the binaries. `which` is used to confirm the binaries are located in the appropriate directory. 


```
cd Downloads/
wget https://bitcoincore.org/bin/bitcoin-core-0.20.0/bitcoin-0.20.0-x86_64-linux-gnu.tar.gz https://bitcoincore.org/bin/bitcoin-core-0.20.0/SHA256SUMS.asc
sha256sum --ignore-missing --check SHA256SUMS.asc
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 01EA5486DE18A882D4C2684590C8019E36C2E964
gpg --verify SHA256SUMS.asc
tar xzf bitcoin-0.20.0-x86_64-linux-gnu.tar.gz
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-0.20.0/bin/*
which bitcoind
```

<img src="https://thumbs.gfycat.com/BlondDecimalAsiantrumpetfish-size_restricted.gif" width="440" height="240">

#### **Create bitcoin.conf**

##### `cd` is used to get to  the home directory. `mkdir` is used to create the directory in which Bitcoin Core's data will exist. `echo` is used to write lines to the `bitcoin.conf` output file ready for use with Liquid. For a more in-depth bitcoin.conf guide, type `bitcoind -help` or use this   [bitcoin.conf generator](https://jlopp.github.io/bitcoin-core-config-generator/ "https://jlopp.github.io/bitcoin-core-config-generator/"). Your Bitcoin Core data directory will be `/home/<username>/.bitcoin`

##### *Don't forget to change your `rpcuser` and `rpcpassword` after the `=` before pasting this codeblock into your terminal.* 


```
cd 
mkdir .bitcoin
cd .bitcoin
echo "server=1
#daemon=1
#regtest=1
#regtest.rpcport=18888
#regtest.port=18889
rpcuser=<REPLACE_USER>
rpcpassword=<REPLACE_PASSWORD>
prune=20000
dbcache=3000 " >> bitcoin.conf
```

<img src="https://thumbs.gfycat.com/DesertedMilkyBarasinga-size_restricted.gif" width="440" height="240">

### **2. Install Elements**

##### `cd` is used to move to the appropriate directory. `rm` is used to delete the SHAsums that were previously used to verify Bitcoin Core. `wget` is used to download the latest Elements release and its SHAsums. `sha256sum` is used to verify the compressed download. `gpg` is used to import the pgp key that signs binary releases and verify the signed, compressed binaries. `tar` is used to extract the binaries. `sudo install` is used to invoke administrator priveleges and then install the binaries. `which` is used to confirm the binaries are located in the appropriate directory.



```
cd
cd Downloads/
rm SHA256SUMS.asc
wget https://github.com/ElementsProject/elements/releases/download/elements-0.18.1.8/elements-0.18.1.8-x86_64-linux-gnu.tar.gz https://github.com/ElementsProject/elements/releases/download/elements-0.18.1.8/SHA256SUMS.asc
sha256sum --ignore-missing --check SHA256SUMS.asc
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 8CC974D9CFD034DCEED213B02A57E0A610D7F19C
gpg --verify SHA256SUMS.asc
tar xzf elements-0.18.1.8-x86_64-linux-gnu.tar.gz
sudo install -m 0755 -o root -g root -t /usr/local/bin elements-0.18.1.8/bin/*
which elementsd
```

<img src="https://thumbs.gfycat.com/IdealSaltyCapybara-size_restricted.gif" width="440" height="240">

#### **Create elements.conf**

##### `cd` is used to get to  the home directory. `mkdir` is used to create the directory in which Elements' data will exist. `echo` is used to write lines to the `elements.conf` output file ready for use with Bitcoin Core. For more info, type `elementsd -help` in your terminal. Your Elements data directory will be `/home/<username>/.elements`

##### *Don't forget to change your `liquidv1.rpcuser`, `liquidv1.rpcpassword`, `mainchainrpcuser`,  and `mainchainrpcpassword` after the `=` before pasting this codeblock into your terminal. Furthermore, `mainchainrpcuser` and `mainchainrpcpassword` must match the `rpcuser` and `rpcpassword` in your bitcoin.conf to use Liquid in production.* 

```
cd 
mkdir .elements
cd .elements
echo "chain=liquidv1
liquidv1.rpcuser=<REPLACE_USER>
liquidv1.rpcpassword=<REPLACE_PASSWORD>
liquidv1.rpcport=18884
prune=10000
#elementsregtest.port=18888
#rpcport=18884
#Over p2p we will only connect to local other elementsd
#elementsregtest.connect=localhost:18887
# will accept incoming connections!
listen=1
# Just for looking at random txs
#txindex=1
# We want to validate pegins by checking with bitcoind if header exists in best known chain, and how deep. We combine this with pegin proof included in the pegin to get full security.
validatepegin=0
# Connect to bitcoind:
mainchainrpcport=18888
mainchainrpcuser=<REPLACE_USER>
mainchainrpcpassword=<REPLACE_PASSWORD>
# Free money to make testing easier
#initialfreecoins=2100000000000000" >> elements.conf
```

<img src="https://thumbs.gfycat.com/FatTalkativeHoneybee-size_restricted.gif" width="440" height="240">

### 3. Begin Syncing
##### `bitcoind` begins syncing the Bitcoin blockchain. It will require its own terminal. `elementsd` begins syncing the Liquid sidechain. It will also require its own terminal. You can stop syncing by hitting `ctrl c`. `getblockcount` RPC is used to monitor sync progress. `verifychain` is used to ensure validity of the chain.

```
bitcoind #in its own terminal, ctrl c to end
elementsd #in its own terminal, ctrl c to end
bitcoin-cli getblockcount
bitcoin-cli verifychain
elements-cli getblockcount
elements-cli verifychain
```

<img src="https://thumbs.gfycat.com/IllegalDisgustingIcterinewarbler-size_restricted.gif" width="440" height="240">