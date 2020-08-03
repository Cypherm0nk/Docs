# **Receieving & Sending Assets**
## **Receiving an Asset**

To receive an asset, an address must be created:


`getnewaddress  <label> <address_type>`

|| Field | Value     |
| :---        |    :----:   |  ---: |
| Label  | Optional        | "<YOUR_LABEL>"|
| Address type  | Optional       | legacy", "p2sh-segwit", and "bech32"     |


To verify transaction related information sent to your address, use the following RPC's:
```
getaddressinfo <address>
getrawmempool
gettransaction <txid>
getblockcount
dumpassetlabels
getbalance
```
`help getnewaddress` - for more information

### **Receiving an asset to a Multisignature address**

1. Create the first address/key
	* `getnewaddress`
	* `getaddressinfo <address_one>`
	* Save resulting pubkey

2. Create second address/key 
	* `getnewaddress`
	* `getaddressinfo <address_two>`
	* Save resulting pubkey.

3.  Create the Multisignature address
```
createmultisig 2 "[\"<pubkey_one>\",\"<pubkey_two>\"]"
```

Note: Addresses can be in the same wallet or different wallets. The resulting multisig address is our 2 of 2 redeem script.


## **Sending an Asset**

Sending an asset is slightly more complex than sending L-BTC using the `sendtoaddress` RPC. Nine of the ten optional fields will need to be explicitly completed with default or desired parameters to send an asset. 


`sendtoaddress  <address> <amount> "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" "assetlabel" ignoreblindfail`
|| Field | Value     |
| :---        |    :----:   |  ---: |
| Address  | Required (string)      | <receiving_address>|
| Amount  | Required (numeric)      | <asset_amount> e.g. .00000001 is one asset     |
| "comment"  | Optional (string)     | (internal purposes) e.g. used for coffee |
| "comment_to"  | Optional (string)       | (internal purpose) e.g. sent to Bob      |
| subtractfeefromamount| Optional (boolean, default=false)       | Pays fee from amount sent, receiver gets less |
| replaceable conf_target  | Optional (boolean, default=wallet_default)       | BIP 125, Opt-in RBF   |
conf_target  | Optional (numeric, default=wallet_default)       | target tx confirmation in blocks   |
| "estimate_mode"  | Optional (string, default=UNSET)       | "UNSET",  "ECONOMICAL", or "CONSERVATIVE" fee estimation     |
| "assetlabel"  | Required (string)      | The asset label or hex to be sent     |
| ignoreblindfail | Optional ((boolean, default=true)       | Return a transaction even when a blinding attempt fails due to number of blinded inputs/outputs     |


Edample transaction:
```
sendtoaddress <addressS> 0.00000003 "" "" false true 1 UNSET <assetid>
```
Transaction details (public perspective):
```
getrawtransaction <your_txid> 1
```

Notice that the asset type is not known in this transaction. Two inputs (Bitcoin and the asset) and 3 outputs (Bitcoin change, the asset, and the transaction fee) are not distinguished from one another.


`help sendtoaddress` - for more information

### **Sendig multiple assets to addresses in a single transaction**

`sendmany "" {"address":amount} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" {"address":"str"} ignoreblindfail )`

Note: Amounts are double-precision floating point numbers.
|| Field | Value     |
| :---        |    :----:   |  ---: |
| Dummy  | Required (string)      | Must be set to "" for backwards|
| Amount  | Required ((json object)      |  "address": amount,    (numeric or string, required) The address is the key, the numeric amount (can be string) in BTC is the value    |
| "minconf"  | Optional (numeric, default=1)     | Only use the balance confirmed at least this many times |
| "comment"  | Optional (string)       | comment      |
| subtractfeefromamount| Optional (json array)       | A json array with addresses- The fee will be equally deducted from the amount of each selected address |
| replaceable conf_target  | Optional (boolean, default=wallet_default)       | BIP 125, Opt-in RBF   |
conf_target  | Optional (numeric, default=wallet_default)       | target tx confirmation in blocks   |
| "estimate_mode"  | Optional (string, default=UNSET)       | "UNSET",  "ECONOMICAL", or "CONSERVATIVE" fee estimation     |
| output_assets  | Required (json object)      | "address": "str",     (string, required) A key-value pair where the key is the address used and the value is an asset label or hex asset ID     |
| ignoreblindfail | Optional ((boolean, default=true)       | Return a transaction even when a blinding attempt fails due to number of blinded inputs/outputs     |


Edample transactions:

1. Send two amounts to two different addresses:
```
elements-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}"
```

2. Send two amounts to two different addresses setting the confirmation and comment:
```
elements-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 6 "testing"
```

3. Send two amounts to two different addresses, subtract fee from amount:
```
elements-cli sendmany "" "{\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\":0.01,\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\":0.02}" 1 "" "[\"1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX\",\"1353tsE8YMTA4EuV7dgUXGjNFf9KpVvKHz\"]"
```