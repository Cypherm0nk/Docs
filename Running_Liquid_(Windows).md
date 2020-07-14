# **Running Liquid (Windows)** 
### **Getting Started with Liquid**

##### This tutorial will instruct complete beginners on how to get started using Liquid (in production) as quickly as possible with the following steps:

1. Download Bitcoin Core
    * Create bitcoin.conf
2. Download Elements
    * Create elements.conf
3. Install & Sync

### Required:
* 30 GB of disk space
* 8 GB of RAM
* Winodws 10 - win64

### **Preliminaries**
##### Please follow the relevant instructions to install necessary tools for the tutorial.

* [GPG](https://gpg4win.org/download.html "https://gpg4win.org/download.html")

* [Bitcoin Core Alternatives for Windows
](https://github.com/bitcoin/bitcoin/blob/master/doc/build-windows.md "https://github.com/bitcoin/bitcoin/blob/master/doc/build-windows.md")

### **1. Install Bitcoin Core**
##### The codeblock below can be copied and pasted into your command prompt.`cd` is used to get to the Downloads directory.  `curl` is used to download the latest Bitcoin Core release and its SHAsums. `certUtil` is used to generate a checksum of the release file you downloaded. `type` is used to display the checksums downloaded. `gpg.exe` is used to import the pgp key that signs binary releases and verify the binaries (if signed).


```
cd %UserProfile%\Downloads &&
start microsoft-edge:https://bitcoincore.org/bin/bitcoin-core-0.20.0/bitcoin-0.20.0-win64-setup.exe && 
start microsoft-edge:https://bitcoincore.org/bin/bitcoin-core-0.20.0/SHA256SUMS.asc &&
certUtil -hashfile bitcoin-0.20.0-win64-setup.exe SHA256 &&
type SHA256SUMS.asc &&
gpg.exe --keyserver hkp://keyserver.ubuntu.com --recv-keys 01EA5486DE18A882D4C2684590C8019E36C2E964 &&
gpg.exe --verify SHA256SUMS.asc
```


![GIF](https://thumbs.gfycat.com/DimwittedInsignificantArthropods-size_restricted.gif)

#### **Create bitcoin.conf**

##### `mkdir` is used to create the directory in which Bitcoin Core's data will exist. `cd` is used to get to the appropriate directory. `echo` is used to write lines to the `bitcoin.conf` output file ready for use with Liquid. For a more in-depth bitcoin.conf guide, please use this   [bitcoin.conf generator ](https://jlopp.github.io/bitcoin-core-config-generator/ "https://jlopp.github.io/bitcoin-core-config-generator/"). Your Bitcoin Core data directory will be `C:\Users\<username>\AppData\Roaming\Bitcoin`.

##### *Don't forget to change your `rpcuser` and `rpcpassword` after the `=` before pasting this codeblock into your terminal.* 


```
cd %UserProfile%\Downloads &&
mkdir C:\Users\%UserProfile%\AppData\Roaming\Bitcoin &&
cd C:\Users\%UserProfile%\AppData\Roaming\Bitcoin &&
echo server=1 > bitcoin.conf && 
echo rem daemon=1 >> bitcoin.conf && 
echo rem regtest=1 >> bitcoin.conf && 
echo rem regtest.rpcport=18888 >> bitcoin.conf && 
echo rem regtest.port=18889 >> bitcoin.conf && 
echo rpcuser=V >> bitcoin.conf && 
echo rpcpassword=MAINpass >> bitcoin.conf && 
echo prune=20000 >> bitcoin.conf && 
echo dbcache=3000 >> bitcoin.conf
```

![GIF](https://thumbs.gfycat.com/DeterminedSaneBlacklab-size_restricted.gif)


### **2. Install Elements**

##### `cd` is used to get to the Downloads directory. `start microsoft-edge` is used to download the latest Elements release and its SHAsums. `certUtil` is used to generate a checksum of the release file you downloaded. `type` is used to display the checksums downloaded. `gpg.exe` is used to import the pgp key that signs binary releases and verify the binaries (if signed).




```
cd %UserProfile%\Downloads &&
start microsoft-edge:https://github.com/ElementsProject/elements/releases/download/elements-0.18.1.8/elements-0.18.1.8-win64-setup-unsigned.exe &&
start microsoft-edge:https://github.com/ElementsProject/elements/releases/download/elements-0.18.1.8/SHA256SUMS.asc
```
```
certUtil -hashfile elements-0.18.1.8-win64-setup-unsigned.exe SHA256 &&
type SHA256SUMS.asc &&
gpg.exe --keyserver hkp://keyserver.ubuntu.com --recv-keys 8CC974D9CFD034DCEED213B02A57E0A610D7F19C &&
gpg.exe --verify SHA256SUMS.asc
```
![GIF](https://thumbs.gfycat.com/MedicalCoolDingo-size_restricted.gif)


#### **Create elements.conf**

##### `mkdir` is used to create the directory in which Elemens'+++ data will exist. `cd` is used to get to the appropriate directory.  `echo` is used to write lines to the `elements.conf` output file ready for use with Bitcoin Core. Your Elements data directory will be `C:\Users\<username>\AppData\Roaming\Elements`.

##### *Don't forget to change your `liquidv1.rpcuser`, `liquidv1.rpcpassword`, `mainchainrpcuser`,  and `mainchainrpcpassword` after the `=` before pasting this codeblock into your terminal. Furthermore, `mainchainrpcuser` and `mainchainrpcpassword` must match the `rpcuser` and `rpcpassword` in your bitcoin.conf to use Liquid in production.* 

```
cd %UserProfile%\Downloads &&
mkdir C:\Users\%UserProfile%\AppData\Roaming\Elements &&
cd  C:\Users\%UserProfile%\AppData\Roaming\Elements &&
echo chain=liquidv1 > elements.conf &&
echo liquidv1.rpcuser=Luser >> elements.conf &&
echo liquidv1.rpcpassword=Lpass >> elements.conf &&
echo liquidv1.rpcport=18884 >> elements.conf &&
echo prune=10000 >> elements.conf &&
echo ren elementsregtest.port=18888 >> elements.conf &&
echo rem rpcport=18884 >> elements.conf &&
echo rem elementsregtest.connect=localhost:18887 >> elements.conf &&
echo listen=1 >> elements.conf &&
echo rem txindex=1 >> elements.conf &&
echo validatepegin=0 >> elements.conf &&
echo mainchainrpcport=18888 >> elements.conf &&
echo mainchainrpcuser=V >> elements.conf &&
echo mainchainrpcpassword=MAINpass >> elements.conf &&
echo rem initialfreecoins=2100000000000000" >> elements.conf 
```

![GIF](https://thumbs.gfycat.com/OffbeatSnivelingIbex-size_restricted.gif)

### 3. Install & Sync
##### `bitcoind` begins syncing the Bitcoin blockchain. It will require its own command prompt. `elementsd` begins syncing the Liquid sidechain. It will also require its own command prompt. You can stop syncing by hitting `ctrl c`. `getblockcount` RPC is used to monitor sync progress. `verifychain` is used to ensure validity of the chain.

```
cd %UserProfile%\Downloads &&
bitcoin-0.20.0-win64-setup.exe &&
elements-0.18.1.8-win64-setup-unsigned.exe
```
![GIF](https://thumbs.gfycat.com/PowerlessLankyGermanshorthairedpointer-size_restricted.gif)
![GIF](https://thumbs.gfycat.com/AlienatedFreeChital-size_restricted.gif)

```
cd C:\Program Files\Bitcoin\daemon &&
bitcoind 

cd C:\Program Files\Bitcoin\daemon &&
elementsd

cd C:\Program Files\Bitcoin\daemon\
bitcoin-cli verifychain && 
bitcoin-cli getblockccount && 
elements-cli verifychain && 
elements-cli getblockcount
```
![GIF](https://thumbs.gfycat.com/BitesizedSelfassuredAfricanpiedkingfisher-size_restricted.gif)

