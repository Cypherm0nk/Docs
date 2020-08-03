# **Asset Issuance on Liquid**
### **Issuing an Asset for use with Blockstream’s Asset Registry**

Liquid is often selected for asset issuance purposes due to its Bitcoin Core derived codebase, native multi signature feature, and range proofs that uphold confidential assets and amonts in transactions outputs less than 67,719,476,736 satoshis.

Issuing an asset can be as simple as `elements-cli issueasset <asset_amount> <reissuance_token_amount>`

### Asset Issuance basics




Before issuing an asset, it’s important to understand its purpose and decide how its characteristics will be adhered to throughout its lifecycle:

####  ***Is the asset issuance intended to be confidential or public information?*** 

An asset would remain confidential if the parties that are intended to use it are few in numbers and have already established trust. On the other hand, an asset should be public if it is expected to be widely distributed.

#### ***How much of the asset will be initially issued?***

Up to 21,000,000 of an asset and reissuance token (at inital issuance) can be issued using `issueasset` or `rawissueasset`, which will be further explained below.

#### ***Will the total supply of the asset ever require decreasing and/or increasing?***

During an asset's lifecycle, decreasing and increasing is possible using `destroyamount`, `reissueasset`, and `rawreissueasset`. Spending of a reissuance token will enable reissuing the corresponding asset. Best practices include cold storage back ups and/or multi signature schemes for reissuance tokens in case of user error or malice.

#### ***Does the asset require divisibility to a specified precision?*** 

We highly recommend always using eight decimal places when issuing an asset.

#### RPC cheatsheet
```
elements-cli help
elements-cli issueasset <asset amount> <reissuance token amount> #OPTIONAL unblinded:false
elements-cli reissueasset <asset> <asset_amount> 
elements-cli destroyamount <asset> <asset_amount> #OPTIONAL comment
echo "assetdir = <assetid>:name
assetdir = <assetid>:name" >> elements.conf # Replace hex inside < > and name with asset at ressuance token
```


### Create an asset for use with Blockstream's Asset registry in the following steps:
       
1. Create the contract hash with issued asset attributes
2. Create and fund a “raw transaction”
3. Blind, sign, and send “raw issue asset” 
4. Submit to Blockstream’s Asset Registry






### 

### Create the contract hash with issued asset attributes

The contract hash represents the information attributes that are committed to about the asset being issued. 



|      | Field | Value     |
| :---        |    :----:   |  ---: |
| Vesrion      | Required       | 0 |
| Issuer_pubkey   | Required        | hex-encoded public key of the issuer      |
| Name   | Required        | 1-255 ASCII characters      |
| Entity   | Required        | domain name      |
| Ticker  | Optional        | 3-5 characters consisting of a-z, A-Z, . and -      |
| Asset Amount  | Optional        | value of 21 will create an amount Blockstream Explorer will show as 21,000,000 |
| Reissuance Token Amount  | Optional        | value of 0.00000001 will create an amount Blockstream Explorer will show as 1|
| Precision  | Optional       | number of digits after the decimal point, i.e. 0 for non-divisible assets or 8 for BTC-like. defaults to 0.     |




```
{
  "version": 0,
  "issuer_pubkey": "037c7db0528e8b7b58e698ac104764f6852d74b5a7335bffcdad0ce799dd7742ec",
  "name": "Foo Coin",
  "ticker": "FOO",
  "entity": { "domain": "foo-coin.com" },
  "precision":8
}
```

The contract hash is the (single) sha256 hash of the contract json document, canonicalized to have its keys sorted lexicographically.
```
CONTRACT='{"version":0,"ticker":"FOO","name":"Foo Coin"}'
CONTRACT_HASH=$(python -c 'import json,sys; sys.stdout.write(json.dumps(json.loads(sys.argv[1]), sort_keys=True, separators=(",",":")))' "$CONTRACT" | sha256sum | head -c64 | fold -w2 | tac | tr -d "\n")
echo $CONTRACT_HASH
```

To verify control of the entity domain , make a file on a webserver available at `https://<domain>/.well-known/liquid-asset-proof-<asset-id>`, with the following contents:
`Authorize linking the domain name <domain> to the Liquid asset <asset-id>`

Note: Serving the file with https is required, except for .onion hidden services. We recommend using github pages to host the ile for rapid testing. 

Prepare your contract json and get your contract hash. Verify their validity before issuing the asset using the validation endpoint:
```
curl https://assets.blockstream.info/contract/validate -H 'Content-Type: application/json' \
       -d '{"contract": <your-contract-json>, "contract_hash": "<your-contract-hash>"}'
```

After confirming everything is correct, `elements-cli rawissueasset` with your hash as the `contract_hash` parameter and take note of the resulting `asset_id.` Add the domain ownership proof (as described above). 





After the issuance transaction confirms, submit the asset to the registry:

```
curl https://assets.blockstream.info/ -H 'Content-Type: application/json' \
       -d '{"asset_id": "<asset-id>", "contract": <contract-json>}'
```
This can also be done using the asset registry CLI utility: 
```
liquid-asset-registry register-asset --asset-id <asset-id> --contract <contract-json>
```

Deleting Asset Registration Data
Deleting an asset from the Blockstream Asset Registry only removes its metadata from the registry. It does not, and cannot, delete the asset itself from the Liquid Blockchain.

To delete an asset from the Blockstream Asset Registry, sign the following message using the `issuer_pubkey` used when registering the asset:

```
remove <asset-id> from registry
```

Then submit the deletion request with the base64-encoded signature:

```
curl -X DELETE https://assets.blockstream.info/<asset-id> -H 'Content-Type: application/json' \ -d '{"signature":"<base64-encoded-signature>"}'
```
