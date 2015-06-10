# 6. Developers

## 6.1 Web Socket API

The OpenBazaar reference client when running listens on a websocket for messages from the web client.  Future versions may use an encrypted websocket listener. Your running client web socket will be available at: 
    
    ws://127.0.0.1:<OpenBazaar_Port>/ws

Every message will consist of a unique id for the request and a results key-value in JSON containing the data that was requested. This is an example of a _peers_ message.

    {"id": 24637, "result": {"peers": [{"hostname": "xxx.xxx.xxx.xxx", "sin": "9xHVXXi2Lv7tcchgTPLFDDF2qdaP44bDD3yXvP", "nick": "Store Name", "reachable": true, "guid": "d5a848a0957838a2707b92cca9741cb71ab432k3", "pubkey": "044dbc7389598f7d5f133fe11fcec58be344e288ff0acba6bd582131cd2d1a7d7fac59fbe0454257d11e5cb00e1945b02d25679f35ce72af77510f32bb8cdd23433", "port": 12345}], "type": "peers"}}


### Messages
The following section outlines all of the messages and formats that the websocket API enables.

* [GUI Messages](https://github.com/OpenBazaar/OpenBazaar/wiki/Web-Socket-API#gui-messages)
* [Market](https://github.com/OpenBazaar/OpenBazaar/wiki/Web-Socket-API#market)
* [Peer to Peer Network](https://github.com/OpenBazaar/OpenBazaar/wiki/Web-Socket-API#peer-to-peer-network)

#### GUI Messages
These messages are used for interacting with the GUI.

##### republish_notify

Example:

    {
        "id": 843536, 
        "result": 
        {
            "msg": "P2P Data Republished", 
            "type": "republish_notify"
        }
    }

##### peers

Provides a list of peers and their metadata. Used in the GUI for constructing the sidebar of markets available.

Example:

        {
            "id": 24637, 
            "result": 
            {
                "peers": [
                            {"hostname": "XXX.XXX.XXX.XXX", "sin": "", "nick": "Default", "reachable": true, "guid": "", "pubkey": "", "port": 12345}, 
                            {"hostname": "XXX.XXX.XXX.XXX", "sin": "", "nick": "Notary", "reachable": true, "guid": "", "pubkey": "", "port": 56023}
                         ],
                "type": "peers"
            }
        }

#### Market
These requests are used to manage your account/market on the network.

##### create_contract

    {
      "id": 42,
      "command": "create_contract",
      "params": {
        "Contract_Metadata": {
          "OBCv": "0.4",
          "category": "physical_goods",
          "subcategory": "fixed_price",
          "contract_nonce": "01",
          "expiration": "2014-01-01 00:00:00"
        },
        "Seller": {
          "seller_GUID": "",
          "seller_BTC_uncompressed_pubkey": "",
          "seller_PGP": ""
        },
        "Contract": {
          "item_title": "asfd",
          "item_keywords": [
            
          ],
          "currency": "XBT",
          "item_price": 0.5,
          "item_condition": "New",
          "item_quantity": 1,
          "item_desc": "asfddasf",
          "item_images": {
            
          },
          "item_remote_images": [
            
          ],
          "item_delivery": {
            "countries": "",
            "region": "",
            "est_delivery": "",
            "shipping_price": 0
          }
        }
      }
    }

##### check_order_count

Request:

    {
        "id":42,
        "command":"check_order_count",
        "params":{}
    }

Response:

    {
        "id": 28796,
        "result": 
        {
            "count": 1, 
            "type": "order_count"
        }
    }

##### update_settings

Request:

    {
      "id": 42,
      "command": "update_settings",
      "params": {
        "type": "update_settings",
        "settings": {
          "countryCode": "",
          "stateRegion": "",
          "guid": "67e53acbd794f702768bb6beca33322120e8d353",
          "id": 1,
          "privkey": "",
          "city": "",
          "notaries": [
            {
              "nickname": "Default",
              "guid": "7a9112bc8f7f7558d75d31420d8a36a5e5ec965b",
              "$$hashKey": "00G"
            }
          ],
          "zip": "",
          "market_id": 1,
          "arbiterDescription": "",
          "secret": "",
          "street1": "",
          "pubkey": "",
          "sin": "Tf71nA7HPeCuiMTxLmSfhGdi2bQWvQqEg8a",
          "storeDescription": "",
          "obelisk": "obelisk-baltic.airbitz.co:9091",
          "namecoin_id": "",
          "street2": "",
          "welcome": "enable",
          "recipient_name": "",
          "arbiter": "",
          "notary": "True",
          "bitmessage": "",
          "PGPPubKey": "",
          "bcAddress": "",
          "country": "",
          "bip32_seed": "",
          "PGPPubkeyFingerprint": "",
          "email": "",
          "nickname": "Default",
          "trustedArbiters": [
            
          ],
          "stateProvinceRegion": "",
          "burnAmount": 0,
          "burnAddr": "1AUMCv6Vo6bUJCHWjayECuyUVLent5apRm"
        }
      }
    }

##### get_notaries

Retrieve all of the trusted notaries for your marketplace.

Request:

    {
        "id":42,
        "command":"get_notaries",
        "params":{}
    }

Response:

    {
        "id": 937628, 
        "result": 
        {
            "notaries": [
                {"nickname": "Default", "guid": "7a9112bc8f7f7558d75d31420d8a36a5e5ec965b", "$$hashKey": "00G"}
            ], 
            "type": "settings_notaries"
        }
    }

#### Peer to Peer Network
These requests are sent to the peer to peer network.

##### hello
Hello messages are one-way notices of presence from one node to another.

Request:

    {
      "id": 66273,
      "result": {
        "senderNick": "Default",
        "senderNamecoin": "",
        "type": "hello",
        "hostname": "127.0.0.1",
        "nat_type": "Restric NAT",
        "senderGUID": "",
        "v": "0.3.1",
        "pubkey": "",
        "port": 12346
      }
    }

##### query_page

Queries information about a specific store by looking against it's GUID.

Request:

    {
        "id":42,
        "command":"query_page",
        "params": {
                      "type":"query_store_listings",
                      "findGUID":"d5a848a0957838a2707b92cca9741cb71ab4a754"
                  }
    }

Response:

    {
        "id": 269057, 
        "result": 
        {
            "senderGUID": "31fa9f7e807aaa2740a7803f7ba48d15f5c5a396", 
            "senderNick": "Default", 
            "bitmessage": "", 
            "senderNamecoin": "", 
            "text": "", 
            "uri": "tcp://127.0.0.1:12345", 
            "arbiter_description": "", 
            "email": "", 
            "nickname": "Default", 
            "arbiter": "", 
            "v": "0.3.1", 
            "notary": "", 
            "guid": "67e53acbd794f702768bb6beca33322120e8d353", 
            "type": "page", 
            "sin": "Tf26hJBmGci2o7rUwKqamiaNLNA6wL1SJSE", 
            "PGPPubKey": "", 
            "pubkey": ""
        }
    }

##### query_store_products
Requesting product listings from another market will return asynchronously all of the contracts it is offering as store_contract messages. It is your responsibility to deal with them as they are received.

Request:

    {
        "id":42,
        "command":"query_store_products",
        "params":
        {
            "type":"query_store_products",
            "key":"31fa9f7e807aaa2740a7803f7ba48d15f5c5a396"
        }
    }

Response:

    {
      "id": 532405,
      "result": {
        "type": "store_contract",
        "contract": {
          "senderNick": "Default",
          "senderNamecoin": "",
          "deleted": 0,
          "type": "listing_result",
          "senderGUID": "31fa9f7e807aaa2740a7803f7ba48d15f5c5a396",
          "unit_price": 0.5,
          "item_remote_images": [
            "http:\/\/i00.i.aliimg.com\/wsphoto\/v0\/1991723914_1\/Free-Shipping-Hot-Sale-Wholesale-Novelty-Items-Funny-Punch-Guns-Kid-s-Favourite-Jokes-for-Fun.jpg"
          ],
          "item_quantity_available": 1,
          "item_title": "Novelty Gun",
          "item_desc": "Toy gun.",
          "signed_contract_body": "",
          "item_images": {
            
          },
          "key": "8def8106bfed395ba39eb626b316edd879349fe5",
          "v": "0.3.1",
          "contract_body": **<RICARDIAN CONTRACT OBJECT>**,
          "item_condition": "New",
          "guid": "67e53acbd794f702768bb6beca33322120e8d353",
          "pubkey": "",
          "id": 379843,
          "shipping_price": 0
        }
      }
    }

##### findNode
This call is used to find specific keys but can also be used to bootstrap your view of the network by passing your own GUID. If a node does not have a value for the DHT key you are searching for they should return a list of closest nodes to that key that they know about.

Request:

    {
        "id": 111970,
        "result":
        {
            "findID": "63307178a21db3ff7e4a2e8d6672e0a0dc52cddc",
            "senderNamecoin": "",
            "type": "findNode",
            "hostname": "XXX.XXX.XXX.XXX",
            "nat_type": "Restric NAT",
            "findValue": false,
            "senderGUID": "a9eb74d66581816f7e4b55ea26ef2b66a57bbf3c",
            "key": "fdf629eedacc5537e749f542241413f15ed7ad94",
            "v": "0.3.1",
            "senderNick": "Notary",
            "guid": "2743a65cd38e43c3aab2957ec5a50514a2ef9322",
            "pubkey": "04422453d12042ec7484c0147bd6deef03575234d62389676ed824583e2e024ae210ad263543841ad47e73b3f08a666eb0655d5efd1cc6dcb728ce76af8173381a",
             "port": 56023
        }
    }

Response (value not found):

    {
      "id": 868758,
      "result": {
        "senderNick": "Default",
        "senderNamecoin": "",
        "foundNodes": [
          
        ],
        "hostname": "127.0.0.1",
        "senderGUID": "31fa9f7e807aaa2740a7803f7ba48d15f5c5a396",
        "v": "0.3.1",
        "findID": "a9572416269500d2adadb44ba36b9fdea6e13d9d",
        "guid": "67e53acbd794f702768bb6beca33322120e8d353",
        "type": "findNodeResponse",
        "port": 12345,
        "pubkey": ""
      }
    }

