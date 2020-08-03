# **Sharing a Blinding Key**
### How to use confidential addresses and keys

Transactions in Liquid have confidential assets and amounts by deafult. In cases in which a third party must audit transactions for accounting or compliance purposes, they can do so by importing a `confidential_address` the `confidential_key`.

1. Create an address with `getnewaddress` and share the resulting `confidential_address` with the third party
2. `dumpblindingkey <confidential_address>` and share the resulting `blinding_key` with the third party
3. Third party must `importaddress <confidential_address>` 
4. Third party then must `importblindingkey <confidential_address> <confidential_key>`
5. Third party can now view the results of  `getaddressinfo <confidential_address>` with asset labels and amounts 

