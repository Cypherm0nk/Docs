# **Testing Applications**

Testing your applications with testnet and regtest can be useful. Your `bitcoin.conf` and `elements.conf` will have to be adjusted for the desired environment. Below are some examples that may serve as starting place. You can always further adjust `.conf` files based on the information given by `bitcoind -help` and `elementsd -help` respectively.

## **Testnet**
#### Bitcoin Core
```
chain=test
daemon=1
txindex=1
regtest.rpcport=18888
regtest.port=18889
rpcuser=user3
rpcpassword=password3
```
#### Elements
```
chain=test
mainchainrpcport=18888
mainchainrpcuser=user3
mainchainrpcpassword=password3
```

## **Regtest**
Block generation can be done by `generatetoaddress <number_of_blocks> <address>`

#### Bitcoin Core
```
regtest=1
daemon=1
txindex=1
regtest.rpcport=18888
regtest.port=18889
rpcuser=user3
rpcpassword=password3
```
#### Elements

```
mainchainrpcport=18888
mainchainrpcuser=user3
mainchainrpcpassword=password3
chain=elementsregtest
validatepegin=1
initialfreecoins=1000000000
```