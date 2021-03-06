# Smart Asset Lifecycle Management

A blockchain smart asset can be the digital representation of a real-world asset (e.g. gold, equity shares, fiat currency, stressed asset) or an asset in itself (e.g. bitcoin and other crypto-currencies). The blockchain removes the characteristic of infinite reproducibility from a digital asset. It confirms that each unit of value was transferred only once, solving the long-standing problem of double spending. A blockchain can assign title rights because it provides a record that compels offer and acceptance. 

[1. Create a new asset](#1-create-a-new-asset)   
[2. Create additional units of an open asset](#2-create-additional-units-of-an-open-asset)   
[3. Details of a specified asset](#3-details-of-a-specified-asset)   
[4. Balances of all assets](#4-balances-of-all-assets)   
[5. Assets held by specified entities](#5-assets-held-by-specified-entities)   
[6. Transfer asset](#6-transfer-asset)    
[7. Transactions by a specified entity](#7-transactions-by-a-specified-entity)    

## 1. Create a new asset
To create a new smart asset, use `post /api/v1/create_new_asset_from`.

***Note:***
*  This may take upto 30 seconds.   
* `from_address` is the entity that creates the asset. It must have `send`, `receive` and `issue` permissions. It must also have permissions to write to the ***ASSET_DATA_MASTERLIST***   
* `to_address` is the entity that receives the newly created asset. It must have `receive` permission and ideally also `send` permission.   
*  The `from_address` and `to_address` can be the same.
*  `unit` refers to the minimum divisible quantity of the asset.
* `asset` refers to the name of the asset.
* `quantity` refers to the initial quantity of the asset.
* `details` refers to any details about the asset.
* Setting `"open": "true"` will create an open asset - additional units of the asset CAN be issued in the future. 
* Setting `"open": "false"` will create a closed asset - additional units of the asset CANNOT be issued in the future. 

```
{
  "from_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  "to_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  "asset": 
  	{
    	"name": "GOLD-tokens-series-A",
    	"open": false
  	},
  "unit": 1,
  "quantity": 19500,
  "details": "Backed by gold jewellery held in Lot A at Zimblia office" 
}

```
***Output is:***
* the txid of the transaction in which the asset was created.
* the reference number of the asset.
* the newly created data stream specifically for writing data relating to this asset. The `to_address` is granted write permissions to this stream. 
```
{
  "status": 200,
  "tx_id": "011e3b965d9156f980980f0dad976a00978ecef4db8a081f166649dd4580a0e7",
  "asset_ref": "301-267-14695",
  "description": 
    {
      "details": "Backed by gold jewellery held in Lot A at Zimblia office"
    }
}
```

## 2. Create additional units of an open asset

To create additional units of an open asset, use `post /api/v1/asset_create_more_from`.

***Note:***
* `from_address` is the entity that creates the asset. It must have `send` permission.
* `to_address` is the entity that receives the newly created asset. It must have `receive` permission.
* The `from_address` and `to_address` can be the same.
* `asset_name` refers to the name of the asset.
* `quantity` refers to the additional quantity of the asset being issued.
```
{
  "from_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  "to_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  "asset_name": "AUTH-tokens-series-A",
  "quantity": 350
}
```
```
{
  "status": 200,
  "tx_id": "0ac10e7cc5d1a3bdd959f74034fabaae7e40ec5e4515aba4c57002e6e50b9736"
}
```
Output is the id of the transaction in which the additional units were created.

## 3. Details of a specified asset
To get details of a specific asset, use `post /api/v1/asset_details` and pass the asset name or referece id as a parameter.
```
{
  "asset_name": "AUTH-tokens-series-A"
}
```
Output:
```
{
  "status": 200,
  "response": 
  [
    {
      "name": "AUTH-tokens-series-A",
      "issuetxid": "d7621cb45b226a4bbf86bde4eb0b7fe700ebfc000f8bb12450362746777dbe44",
      "assetref": "324-267-25303",
      "multiple": 1,
      "units": 1,
      "open": true,
      "details": {
      "details": "These are AUTH tokens"
    },
      "issueqty": 45350,
      "issueraw": 45350,
      "subscribed": true,
      "synchronized": true,
      "transactions": 2,
      "confirmed": 2,
      "issues": 
        [
          {
            "txid": "d7621cb45b226a4bbf86bde4eb0b7fe700ebfc000f8bb12450362746777dbe44",
            "qty": 45000,
            "raw": 45000,
            "details": 
              {
                "details": "These are AUTH tokens"
              },
            "issuers": 
              [
                "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2"
              ],
            },
          {
            "txid": "0ac10e7cc5d1a3bdd959f74034fabaae7e40ec5e4515aba4c57002e6e50b9736",
            "qty": 350,
            "raw": 350,
            "details": {},
            "issuers": 
              [
                "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2"
              ],
          }
        ],
    }
  ],
}
```

## 4. Balances of all assets
To get the name, asset ref and balance of all assets, use `get /api/v1/asset_balances`

```
{
  "status": 200,
  "asset_balance": 
    [
      {
        "name": "AUTH-tokens-series-A",
        "assetref": "324-267-25303",
        "qty": 45350
       },
       {
        "name": "GOLD-tokens-series-A",
        "assetref": "301-267-14695",
        "qty": 19500
        }
    ],
}
```

## 5. Assets held by specified entities
Use  `post /api/v1/assets_held_by_entity` to get details of Assets held by specified entities
```
{
  "primechain_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2"
}
```
```
{
  "status": 200,
  "response": 
    {
      "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2": 
        [
          {
            "name": "AUTH-tokens-series-A",
            "assetref": "324-267-25303",
            "qty": 45350
           },
          {
            "name": "GOLD-tokens-series-A",
            "assetref": "301-267-14695",
            "qty": 19500
          }
        ],
  "total": 
    [
      {
        "name": "AUTH-tokens-series-A",
        "assetref": "324-267-25303",
        "qty": 45350
      },
      {
        "name": "GOLD-tokens-series-A",
        "assetref": "301-267-14695",
        "qty": 19500
       }
    ],
  }
}
```
You can also enter multiple addresses:
```
{
  "primechain_address":
  	[
  		"1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  		"16H6WdDR7LpHzVeEkdTiE6dNiPHBniPtnxTYdd"
  	]
}
```
Output
```
{
"status": 200,
"response": {
"1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2": [
  {
"name": "AUTH-tokens-series-A",
"assetref": "324-267-25303",
"qty": 45350
},
  {
"name": "GOLD-tokens-series-A",
"assetref": "301-267-14695",
"qty": 19460
}
],
"16H6WdDR7LpHzVeEkdTiE6dNiPHBniPtnxTYdd": [
  {
"name": "GOLD-tokens-series-A",
"assetref": "301-267-14695",
"qty": 40
}
],
"total": [
  {
"name": "AUTH-tokens-series-A",
"assetref": "324-267-25303",
"qty": 45350
},
  {
"name": "GOLD-tokens-series-A",
"assetref": "301-267-14695",
"qty": 19500
}
],
}
}
```

## 6. Transfer asset
To send assets from one entity to another use `post /api/v1/send_asset_from` and pass 4 parameters - `from_address`, `to_address`, `asset_name`, `quantity` and `details`
```
{
  "from_address": "1N9VtvZvP3rsw5Rf4Qpi12TWBaDoEwM2BAEsv2",
  "to_address": "16H6WdDR7LpHzVeEkdTiE6dNiPHBniPtnxTYdd",
  "asset_name": "GOLD-tokens-series-A",
  "quantity": 40,
  "details": "These are the details of this transfer"
}
```
```
{
  "status": 200,
  "tx_id": "43dbacfa1af8a27af648de957d7d83b74c675027c8a44b650eb69a8f1cc2406c"
}
```
The output is the id of the transaction in which the asset was transferred.

## 7. Transactions by a specified entity

To list information about the transactions related to an address in this node’s wallet, including how they affected that address’s balance. use `post /api/v1/entity_transaction_list` and pass the address as a parameter.

```
{
  "primechain_address": "16H6WdDR7LpHzVeEkdTiE6dNiPHBniPtnxTYdd"
}
```

```
aaaa
```
