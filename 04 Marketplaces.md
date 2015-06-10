# 4. Marketplaces

**Trade verticals** in *OpenBazaar* refers to types of markets that can be built on top of the platform. In the theory section of the [Github Readme](https://github.com/OpenBazaar/OpenBazaar), I have attempted to describe in minor detail how different types of markets can be built on the platform. These include:

1. Physical goods
    - Fixed price
    - Auctions
2. Digital goods
3. Services
    - Invitation to tender
    - Auction 
    - Fixed rate
4. Lending
    - P2P lending
    - Bonds
5. Financial securities
    - Stocks
    - Futures (bounded futures) 
    - Options
    - Swaps
6. Crowd funding
7. Insurance
8. Currency exchange
9. Barter
10. Prediction markets 
11. Other

<img src="http://s25.postimg.org/9qy5zjaa7/Open_Bazaar_Stack.jpg" />

One question we are often asked is 'can *OpenBazaar* support X market?'... The answer is **yes** if the terms and conditions can be fully represented in a RC and Bitcoin is being used for the trade in exchange or to collateralise an exchange. 

The challenge for *market developers* is to design RCs that capture the semantic detail of a trade to:

1. Establish consensus between the trading parties
2. Prevent all reasonable foreseeable disputes, based on *hearsay*
3. Efficiently process on the OB network between the trading parties (i.e. an efficient signing order) 

To incentivise good performance by each party, surety bonds can also be written into the contract. Surety bonds essentially reward or penalise a trading party based on performance criteria, which would be clearly stated within the RC. 

## 4.1 Physical and Digital Ecommerce

### 4.1.1 Fixed Prices

_Pending_

### 4.1.2 Auctions

It is becoming increasingly clear that *OpenBazaar* will become a powerful platform that will support a variety of peer-to-peer market transactions. One of the most fundamental market transaction types is the *auction* of a good by a seller to discover the market price. In this article, we cover one possible way of auctioning a good using *Ricardian contracts*. 

It should be noted that this proposal essentially describes the 'back-end' of implementing Ricardian contracts into auctions on *OpenBazaar*. It is important that the end-user (i.e. your mother, grandfather etc with novice skills) will be unaware of digital signing, hashing etc occuring in the background unless they choose to be, in order to lower the technical barrier of entry for using *OpenBazaar*.

The merchant firstly selects an *auction* template contract in *OpenBazaar*. In this type of contract, the product and auction details of the good to be sold are entered into *required* fields within the JSON or XML file. The product auction details would include:

1. The name and description of the item
2. A minium sell price, if any
3. A 'buy now' price, if desired
4. An expiration date for the auction
5. A blank field for a bidder to enter their price

For example, let's imagine Alice wants to sell a yellow pinata on *OpenBazaar*. After digitally signing it, the **auction_contract** looks something like this:


```JSON
{
   "001" : {
      "listing" : {
         "id" : {
            "contact" : {
               "bitmessage" : "",
               "email" : "alice@alice.com",
               "subspace" : ""
            },
            "guid" : "zxc987",
            "handle" : "alice",
            "onename" : "+alice",
            "pubkeys" : {
               "bitcoin" : "xxx",
               "pgp" : "xxx"
            },
            "role" : "merchant"
         },
         "item" : {
            "condition" : "Good.",
            "description" : "Good condition, never busted.",
            "images" : {
               "image_hashes" : [
                  "a6e68ab77cb7ca58a8b7e96d3968ad2a4858cca2b6c26e2814c8cb10e50cb00e"
               ],
               "image_urls" : [
                  "https://img0.etsystatic.com/000/0/6342324/il_570xN.326696084.jpg"
               ]
            },
            "keywords" : [ "pinata", "yellow" ],
            "price" : {
               "buy_now" : 2,
               "min_sell" : "0.8"
            },
            "shipping" : {
               "est_delivery" : "14 days",
               "region" : "worldwide"
            },
            "title" : "Yellow pinata"
         },
         "metadata" : {
            "category" : "physical good",
            "category_sub" : "auction",
            "expiry" : "30 days",
            "item_code" : "qwerty",
            "obcv" : "b5.0"
         },
         "notary" : {
            "guid" : "abc123",
            "handle" : "Acme Notary Service"
         }
      },
      "signatures" : {
         "bitcoin" : "XXX",
         "pgp" : "XXX"
      }
   }
}
```

<img src="https://img0.etsystatic.com/000/0/6342324/il_570xN.326696084.jpg" width="100px"/>

Bob accesses the contract and chooses to bid on the pinata, appending his ID details and the bid price. Alice may receive five different bids on the yellow pinata and according to her conditions, the originally contract is updated with the latest bid price. At the end of the expiration date of the contract, Alice digitally signs the final winning **bid contract**, sends it to the arbiter for digitally signing and creating a multisignature bitcoin address for the winning bidder, Bob. 

Due to the architecture of the P2P network setup, Alice may not be available 24/7 during the term of her **auction contract** to update the market on the latest bid price of the item. As a result, she may choose to upload her contract on a 24/7 accessible node (a **negotiator node**) that acts as a contract *server*. If so, the **negotiator node** will create their own contract for the item, digitally sign **bid contracts**, update the `"current_bid"`, and turn over the final **bid contract** to Alice for her digital signature at the end of the contract/auction expiration date. If Alice digitally signs the contract, the contract is sent to an arbiter for sigining and creation of the multisignature bitcoin address for Bob, the winning bidder, to send funds to. As a reward for hosting the contract, the **negotiator node** is rewarded by a fee paid for by Alice (via another multisignature address setup with anoter arbiter). If Alice disapproves of how the contract was negotiated by the **negotiator node** on her behalf, she can simply refuse to sign the contract and raise a dispute to the arbiter for a refund of the fee from the multisignature address.

#### Insurance

For the seller (Alice), she can have confidence that the funds for the item actually exist once Bob has sent the required amount of bitcoins to the multisignature bitcoin address, upon which she will ship the item to Bob's designated address. Similarly for Bob, he can retrieve the funds from the multisignature address if he can prove to the arbiter's satisfaction that he goods either did not arrive, or did not arrive in the condition specified in the contract.

To further promote good behavior, the arbiter may require a **surety bond** from one or both parties. The surety bond is a quantity of bitcoins sent from either the buyer or seller (or both) held in a multisignature bitcoin address that is refundable upon a successful trade. If a dispute arises, and one party is found to be at *malicious* fault, the funds within the surety bond are transferred to the arbiter and opposing party as compensation.

A *non-malicious* versus *malicous* fault may be, for example, a shipping comapany damaging the goods during transport (*non-malicious*) versus the seller knowingly sending a damaged good (*malicious*). The value of the surety bond is negotiable and completely optional. However, it is reasonable to expect that surety bonds will become less necessary between individuals with a high *web of trust* reputation and/or *proof of burn* identity.

## 4.2 Services

A service contract in *OpenBazaar* replaces a physical good to be sold with the terms and conditions of a service to be performed by one party. The distinction between a good and service within a Ricardian contract is minimal. Morever, the combination of reputation management and surety bonds can ensure a robust service industry within *OpenBazaar*.

### Service Contracts

The final format of a service contract is of course comparable to that of a physical good. Similar to physical goods, there are several discovery mechanisms for services on *OpenBazaar*.
For the sake of simplicity, we will examine two types of service contracts that can be supported in *OpenBazaar*, following the Ricardian contract model:

1. Invitation to Tender (ITT)
2. Service Listing

### Invitation to Tender (ITT)

> A call for bids, call for tenders, or invitation to tender (ITT) (often called tender for short) is a special procedure for generating competing offers from different bidders looking to obtain an award of business activity in works, supply, or service contracts... Open tenders, open calls for tenders, or advertised tenders are open to all vendors or contractors who can guarantee performance. (Wikipedia)

The first type of contract is an **invitation to tender (ITT)**, whereby a client publishes their list of requirements for a desired good or service. In the context of a service, these details are specified within a Ricardian contract and distributed to potential service providers in the *OpenBazaar* network. Relevant ITT contract details include:

1. Name and category of the service
  - Chosen from a list of pre-determined categories and sub-cateogories
  - This list will be frequently updated to reflect market needs, and include custom categories to support new markets on *OpenBazaar*
2. Price range
  - The price that the buyer is prepared to pay for the service
  - This can be left blank
3. Estimated time of completion
  - The maximum time that a buyer expects the service to be performed
4. Comments and special considerations
  - Any comments or special details that the service provider needs to be aware of

The details of the contract can be further negotiated between the parties before a contract is double-signed (i.e. a contract signed by both the buyer and service provider indicating consensus on the final version of the contract)

For example, if Alice wanted to deliver some cupcakes to a co-worker on the other side of the city, she writes the following Ricardian-contract as an ITT:

**Contract Hash:** ```5a13604e59b03c3b34f830c53919e176aa4cdc59```

```JSON
{
   "001" : {
      "listing" : {
         "id" : {
            "contact" : {
               "bitmessage" : "",
               "email" : "",
               "subspace" : ""
            },
            "guid" : "",
            "handle" : "",
            "onename" : "",
            "pubkeys" : {
               "bitcoin" : "xxx",
               "pgp" : "xxx"
            },
            "role" : "customer"
         },
         "metadata" : {
            "category" : "service",
            "category_sub" : "invitation to tender",
            "contract_code" : "123-xyz",
            "expiry" : "3 hours",
            "obcv" : "b5.0"
         },
         "order" : {
            "comments" : "Hi, I'd like to deliver the cupcakes to my co-worker before she gets into work, which is around 9.30 am. Thanks.",
            "date" : "2015-07-04 9 am ",
            "delivery_address" : "900 W Eddy St, Chicago, IL 60613, United States",
            "item" : "chocolate cupcake",
            "pickup_address" : "1060 W Addison St, Chicago, IL 60613, United States",
            "quantity" : 1,
            "type" : "delivery"
         }
      },
      "signatures" : {
         "bitcoin" : "XXX",
         "pgp" : "XXX"
      }
   }
}

```

Alice publishes the unencrypted contract to the *OpenBazaar* network, where service providers can scan for contracts that match the category/sub-category fields that they are interested in. Bob, owner of 'Raven Drone Courier', is interested in securing the contract. In order for a service provider to place a bid on a contract, they prepare a 'tender' that will append the following details to Alice's contract:

1. ID details of the individual/business
2. Fee for the service
3. Terms and conditions for providing the service

The contract is then sent to Alice. Alice may receive tens or hundreds of bidding contracts to her **ITT** and can filter the bidding contracts according to their price, delivery time, reputation etc. Once Alice chooses the winning bid contract, and provided she has not finer details to negotiate, she digitally signs contract and sends copies to the bidder and an arbiter to setup the multisignature address. 

#### The *TradeNet* 

A long term outcome of supporting **ITT** contracts within *OpenBazaar* is the potential for it to become a 'TradeNet', as descibed by [Mike Hearn](http://www.slideshare.net/mikehearn/future-of-money-26663148) (slide 25). The original proposal imagined the *TradeNet* to be a market infrastructure for applications, potentially distributed autonomous corporations/businesses, to interface with in order to purchase goods and services according to their programmed goals and profit motives. These applications would issue tenders for goods or services on the *TradeNet*, receive bids and automatically purchase contracts based on algorithmically-determined conditions (factoring in price, proximity, time etc).

A much more attractive alternative is for *OpenBazaar* to potentially faciliate **ITT** contracts for both individuals and distributed consensus/autonomous organisations for peer-to-peer exchanges of goods and services. This is achievable using Ricardian contracts as they are both human and application readable, permitting human-application exchanges without either party known the true identity of each other if so desired.

### Service Listing

The second type of service contract is called a **service listing**, where service providers can advertise their services to potential clients in the hope of receiving a quotation request. Contrary to an *ITT*, where service providers seek out contracts to bid on, a **service listing** will be 'Yellow Pages' of individuals/organisations filtered according to the category/sub-category of services they provide. The **service listing** will be formatted as a Ricardian contract including the following fields:

1. Category and sub-category of service provision
2. Estimated prices
3. Estimated time to complete certain services
4. Availability
5. Comments and special considerations
6. Quotation response time

A potential client can request a quote from the a service provider by drawing up a fresh service contract (according their requirements) that includes a hash of the **service listing**. The inclusion of the hash indicates that the client is requesting a quote based off the specifications advertised by the service provider's **service listing**. From here, the contract is negotiated between both parties as described above prior to double and triple digital signing to initate the contract.

### Good Performance Bonds

A surety bond can be created to cover a failed contract, which either partially or fully remediates the costs of the failed contract for the damaged party. However, as some conditions of the contract can fail without damaging the final outcome of the contract, smaller value surety bonds can be created to incentivise 100% contract fidelity. These smaller surety bonds can be called 'good performance bonds' (GPB). 

For example, GPB may be written to penalise a service provider or supplier for failing to meet a contractually obligated deadline. Another example is a GPB for the client to follow the terms and conditions of the service specified in the contract (i.e. be at a certain place by a certain time).

The GPB can be factored as a deduction for the service provider's fee for the sake of simplicity, which can be carefully monitored by the arbiter. Within the contract, a GPB can be formatted within the following fields (using the example above):

```JSON
{
  "good_performance_bond": {
    "condition_1": {
      "penalty": "7 mBTC",
      "description": "Failure to delivery goods before 9 am"
    },
    "condition_2": {
      "penalty": "5 mBTC",
      "description": "Failure to sign for delivered goods within 7 minutes of the drone's arrivate at the delivery address"
    },
    "condition_3": {
      "penalty": "2 mBTC",
      "description": "Failture to respond to enquires within 20 minutes"
    },
    "condition_4": {
      "reward": "5 mBTC",
      "description": "Item is delivered before 8.45 am"
    }
  }
}
```

The inclusion of GPBs would occur from the outset of the contract's formation, requiring both parties to digitially sign to indicate their agreement. The arbiter can verify the authenticity and integrity of these terms by both party's digital signatures and contract hashes, as per a normal Ricardian contract.

## 4.3 Lending



## 4.4 Financial Securities

#### Bounded Futures

A forward contract is an agreement to buy an item for an agreed price in future. For example it might be possible to agree to purchase a barrel of crude oil a month from now for $100. If in a month the market price of crude is higher than $100, then the contract has positive value. If the price is lower, the contract has negative value.

`Value of bought futures contract in one month = item price in one month - futures price`

Symmetrically, it is possible to enter a contract to sell crude in a month for $100. This gives the negative value of a bought future.

`Value of sold futures contract in one month = futures price - item price in one month`

The contract allows both sides to have an exposure to the market price of crude without owning the asset. For a fungible asset like oil it is not even necessary for to exchange the physical asset at the agreed sale time; buyer and seller can simply settle for the economic value of the contract (difference between the agreed futures price and the market price).

Futures contracts are widely used for currencies, equity indices (like the S&P 500), interest rates, bonds, commodities, and recently for more exotic risks like temperature and rainfall. These markets allow a far greater flexibility in speculation and hedging for market participants than would be available in the ordinary cash markets for the underlying assets. In many cases the futures market volume exceeds the cash market, and the price is first determined by trades in the futures market and later reflected in changes in the cash (or spot) price. 

A difficulty with futures contracts is that it requires trust between contracted parties to pay the agreed contract value. If the price of crude increases to $150 in one month the buyer of the contract must have a mechanism to receive $50 from the seller (or equivalently a physical barrel for $100).  This obligation must be backed by enforceable contract.

Futures traded on financial exchanges have a smart mechanism to solve this problem. Each side provides some amount of money to the exchange as collateral for their side of the agreement. If the market price of the contract changes (observed through other trades on the same item) one side has to provide additional money to the exchange. If they are unable to meet this call for extra collateral the exchange finds a new counterparty to the trade (buyer or seller). If the exchange is careful to always have sufficient collateral to absorb market price movements the contract will require no trust between buyers and sellers. Both sides do not even need to know about each other; the exchange is the legal counterparty to both sides. 

This very elegant solution has a problem: the exchange must monitor the markets and make a guess at the probable size of price changes to determine how much collateral to require from each side. If the price movements are large or the contract is not traded often the exchange might not be able to find a suitable counterparty and be forced to take a financial loss on the contract. **For a properly decentralized market a different solution is required that does not need monitoring of an exchange and also does not expose the parties to credit risk.**

*One solution* is to have contracts that have limited losses or gains. Taking the example of the crude oil contract from the example earlier, a contract might only be exposed to the price risk if the price of crude was between $80 and $120. Each side of the contract would put up $20 in collateral and the value of the contract would be determined without any credit risk or need for margining. 

The payoff of this new contract is a bounded version of a usual futures contract payoff:

<img src="http://s14.postimg.org/3u58am0xt/Equation_1.png" width="400px"/>

(This assumes that the futures price is $100. A slightly more general contract payoff includes an offsetting amount if the price is different.*)

This bounded contract is more flexible than an ordinary future (an ordinary futures contract is a bounded contract with bounds at 0 and ∞). A series of contracts can be "chained" together, where the lower bound of one contract is matched to the upper bound of another. It is also possible to mimic the operation of a traditional futures exchange. As the asset price approaches the upper bound the exchange asks for additional collateral from the seller to buy the next bounded contract. In this way the operation of bounded contracts can be hidden from market participants who find trade in the bounded contracts too daunting.

A more subtle advantage of bounded contracts comes from their non-linear structure. A bounded contract is equivalent to buying a European call option with a strike at the lower bound and selling a call at the upper bound. Hence any market trading bounded contracts is also indirectly pricing and trading options. Usually it is relatively difficult to establish an options market in an asset because it requires sophisticated sellers to price the risk and a high volume in the underlying market for hedging.  This makes options markets expensive and The existence of options in a market provides a much richer set of trading possibilities than with a simple cash or futures market. Options implicitly price expected volatility in the market (this is the essence of the Black Scholes formula – that hedging costs for options are dependent almost completely on the price volatility and not on the price direction). Options also implicitly provide a market estimate of the probability of the price entering a certain range. Imagine a bounded contract with a very small bound (say 99.5 to 100.5). This contract would be worth $0if the price is less than 99.5 and $1 if the price is more than $100.5. The market price of this contract is implicitly an estimate of the probability of the price exceeding $100.5.  If the price of the contract is $0.5 the market believes the probability is 50%; if the price is $0.1 the market believes the probability is 10%.**  

This allows traders to express more sophisticated views on the future price behavior than the crude up/down bet available through a future. The trader can speculate or hedge on the price achieving a certain range, or construct a trade to bet that the market priced probabilities are too wide (variance), or not reflecting sufficient asymmetry in price behavior (skewness). All these additional possibilities enhance the capability of the market to provide effective risk transfer.

However good things do not come for free, and there is a reason why spread contracts are not traded as frequently as futures on exchanges. The existence of multiple contracts divides the market volume between the contracts and makes trading in large size more difficult and costly.  This is a serious drawback but there are mechanisms to moderate the problem. When a contract is offered for sale or purchase some traders might be willing to “convert” the contract into a nearby bound, simultaneously buying the one contract and selling the other. In this case a contract bid or offered at one bound would immediately and automatically provide volume to nearby bounds. This conversion service could be provided by the exchange itself, or by dedicated specialized market makers who would propagate liquidity amongst contracts in the hope of receiving up a small profit on average. 

Bounded futures contracts are natural structures for decentralized markets where trading volumes are likely to be patchy and trust difficult to establish. Parties to the contract can determine how much risk they want to be exposed to and for how long. This removes the usual problems of margining (collateral) from the exchange in exchange for creating a problem by dividing liquidity between multiple contracts. But on net this trade is worthwhile: it allows a completely decentralized and autonomously operating futures market and an extraordinary flexibility of financial structures.

##### Example of a bounded futures contract as a hedge for a sale contract

Suppose we have the following scenario:
 
Chair spot price: 0.01BTC
BTC spot price: $300
One month futures price for BTC $300.
Chair in USD: $3
 
We agree to buy the chair to receive 0.01BTC in one month and wish to hedge the USD/BTC exposure.
 
To hedge with a normal futures contract, we agree to sell 0.01BTC in one month (short selling - shorting - bitcoin) for the futures price of $300/BTC ($3). Any changes in the price of BTC are fully hedged.
 
To hedge with a bounded futures contract we agree to sell 0.01BTC in one month for the futures price of $300/BTC ($3) bounded between 250USD/BTC ($2.5) and 350 USD/BTC ($3.5).
 
If the price of BTC after a month is at the upper bound ($350), the contract to sell 0.01BTC for $300 ($3.5) has a market value of -$0.5 (i.e. $3 - $3.5 = -$0.5). At the lower bound ($250)  the contract is worth $0.5 (i.e. $3 - $2.5 = $0.5).
 
Both parties to the bounded futures contract put up $0.5 in collateral, in the multisignature escrow address, to cover their the maximum possible loss.
 
If the price of BTC/USD varies less than 50% the bounded futures contract provides a perfect hedge.  If it moves more in either direction (above $350 or below $250) then the hedge is only partial.

```
(*) This is summarised by the following:

<img src="http://s28.postimg.org/6019zqdwd/Equation_2.png" width="400px"/>  
where F is the bounded contract "price" which makes the contract a fair trade for both parties. A more general form is:  
<img src="http://s9.postimg.org/w954bx9wf/Equation_3.png" width="200px"/>  
(**) In the limit as the bound approaches zero this is known as the “risk neutral” probability. This allows a probability distribution (the “risk neutral distribution”) to be constructed across the range of price where the contract trades.
```

#### 2. Bounded Futures Ricardian Contract for *OpenBazaar*

This is an example of a bounded futures contract that may be issued in *OpenBazaar*:

```
{
  "001": {
    "listing": {
      "metadata": {
        "obcv": "0.1",
        "category": "Securities",
        "subcategory,": "Bounded Futures",
        "Nonce": "XXX-XXX",
        "Expiration": "XXXX-XX-XX XX:XX"
      },
      "futures_contract": {
        "item": "Wheat",
        "units": "30 bushels",
        "current_price_per_unit_btc": " 0.02",
        "price_total_btc": "0.6",
        "term_days": "5 days",
        "limit_btc": "0.05",
        "spot_price_source": "http://data.tradingcharts.com/futures/quotes/W.html"
      },
      "issuer": {
        "guid": "b5016e193656d33693a38bc2dbea7c80440c09ee",
        "handle": "drwasho",
        "legal_address": "",
        "pubkeys": {
          "pgp": "xsFNBFMZcEIBEADJlq0oVgLfFDdW9WOBguskPdSSeAfHe4s9w8QlmRuO/Zj548gKfofbM84rtP3rHSOSkeOTsu5GwDt48V/md6gyJ69BTZJkJ6qmxFtGaWVRLP/UD4mavW4EAn3PvWY6X7Z8x36U3j2I6vknH1Ufu5Dh5qvQC3WsMliul9ZxlJQZ1/TkQE+qI/gBPRmsMFZ/xV2VOEjMtM3qOPoemhYFzU39/ra0isk81sXrotySkvWw6zsrx8NrkBaw9mxhs0kumF06AfKSrBjytFIdbJaWtNrCdxJ+NMfzQUHmq9bzpBI2VpNJzFQI4WlO8eRYDS1Z88VxXOjMZchd70JfNNWcXwCUeOA/HUgHO4tWczvb/5/C5pfZRKuDqfOLntcMEzpPxbhAbXvU+K5TK394SGgu9ioXl3rdrFj1B1EBPH2BfnOdwCiOOr5ccWVPUHdG34i4D98ARgeqEDxq9WZwq8FG9rgSszqkmnGQtSZxz8aoW1kU1h1SQumqs2ZUrEoLGACkNg16kRq2z7RBLc4MAm3/GW3ygeEFQxK0PMief0X2+l0oVo7a0ARMLLLx4ckuoX3DIJvBegFPvlp0rO3JihdzNmCqIBTDok7D0k2NiQsm3WDJHAYB+8G4ruHVPAOzPNxz5krF+u71jcWmZKO4FRAcXCrWTQCku7wLyzOF80sVEDlwARAQABzWNEciBXYXNoaW5ndG9uIFlhbWFuZHUgU2FuY2hleiAoMjAxNCBLZXlwYWlyIGZvciBuZXcgZW1haWwgYWRkcmVzcykgPHdhc2hpbmd0b24uc2FuY2hlekBvdXRsb29rLmNvbT7CwXkEEwECACMFAlMZcEICGw8HCwkIBwMCAQYVCAIJCgsEFgIDAQIeAQIXgAAKCRDgeMiuv/IyzzaGEACiYWINH5utgquhNPIGl8oPXQwi3cdHbLROXk9s8K5jumxO3qyzrZuag+M39eKrDOgMTteLcCuJV+9sgdmSJHsLuH9o9/PPwaKHsY995C0Y5ZRsvABRQbHoOJPsdq136gH2Z0kW/RJ2fqQfNrDiqyPJpWTkrnnRZF2XU8OlIAq+BsyeVqxOiht1vMIz2TXLanbP6leas7Wah1GRuzhd68+NCyCUQJ4bnBDv6cbGwXRVi7kOOvXYoTilCENdbSuYfGWrq7z54DrefjTP3+0Y/YTWwUQr/yvXi915UCDVU+T6jaqCwtqmezr1VjkMrFQOOrOY7jWmYo4AwQfr4hW9iLdiggr6bPmz0YRfT83a9r9Z+SmU4zuLWmmJXARiUGBwhXUmyqFQa1yjCVI+50uW8Wx3iDot9qiO7gSB878Zq1sKtt3EXolTDGgpVIxZ78do42X02wD8gwGGOdDU3Dzywjy3qcIbVWreuT8tEvCU+Wxw0TGzeoBLcHezhLEbaAOmE5jAmKPL9EbRQBGS1nV3uwzW24vu9ftNqmseYXC2afWDCulmUHHAQEfleJA/mKii2mYV7Yxclib81+EzkzJBPijgUJBN9A1PxqliF8ZDXF/h/jQomASoDBAL8VWo7qwTQlc5CZKZ6xnUZGdUU9lFBmnclw0A6PrMBaZjm1zeKJr1Q===G1fD",
          "bitcoin_pubkey": "044448c02963b8f5ba1b8f7019a03b57c80b993313c37b464866efbf61c37098440bcdcc88bedf7f1e9c201e294cf3c064d39e372692a0568c01565b838e06af0b"
        }
      }
    },
    "signatures":{
      "pgp": "XXX",
      "bitcoin_key": "XXX"
    }
  }
}
```

_Following the creation of the contract, the buyer would submit a bid offer on the contract. Involvement of the notary along with digitial signatures and counter-signatures would proceed as per normal._

Above, `drwasho` is locking in the purchase (in 5 days) of 30 bushels of wheat at a *current* price of 0.02 BTC per bushel (total price: 0.6 BTC). As this is a *bounded futures contract*, `drwasho` places a limit on his exposure to price volatility equal to 0.05 BTC. In other words, `drwasho` is going long on wheat, setting a maximum profit/loss margin of 0.05 BTC. If in 5 days the price for 30 bushels is > 0.65 BTC, `drwasho ` will withdraw 0.1 BTC from the multisignature escrow (his original 0.05 BTC and the 0.05 BTC from the losing counterparty). If the price is < 0.55 BTC, `drwasho` will lose 0.05 BTC from the multisignature escrow address. Remember, the intention of these contracts are to hedge against price volatility, not necessarily trade the full value of the commoditity to be hedged!

## 4.5 Distributed Currency Exchange

*OpenBazaar* allows for the decentralised exchange of types of currencies using the flexible Ricardian contract system. Currency exchanges are not limited trades between crypto-currencies, but can also facilitate exchanges with fiat currencies using reverisble and non-reversible payment systems. 

Fundamentally, currency exchanges require a matching of buy and sell orders at a certain price for a given volume. Firstly, buy and sell orders will be created and issued as a Ricardian contract, formatted according to a specialised 'currency' template in *OpenBazaar*. Secondly, matching buy and sell orders theoretically will be mediated over *exchange nodes*, which may also function as arbiters for each exchange. Alternative, buy and sell orders may be matched over the *OpenBazaar* distributed hash table. Finally, private exchanges can be made between peers, using *OpenBazaar* to merely faciliate the signing/counter-signing of the contract and finding an arbiter for the trade. After matching buy and sell order, the use of Bitcoin multisignature transactions is the key to managing counterparty risk for an exchange between different crypto-currencies, irrespective of whether Bitcoin is the final currency to be exchanged. 

Currency contract can be further sub-categorised into:

1. Crypto-Crypto currency exchanges
2. Crypto-Fiat currency exchanges
3. Fiat-Fiat currency exchanges

### 1. Crypto-Crypto Currency Exchanges
To create a currency Ricardian contract for a crypto-crypto exchange, a *seed contract* is prepared with the following data fields:

1. Crypto-currency pair (e.g. Litecoin/Bitcoin; LTC/BTC)
2. Exchange rate for the currency pair (e.g. 0.01637)
3. Type (i.e buy, sell)
4. Size (i.e. amount of the currency to buy/sell)
5. Payment address

For example, Alice desires to purchase 5 litecoin for a price of 0.01637 LTC/BTC (0.08185 BTC total). She draws up the following *seed contract* and broadcasts it on *OpenBazaar*:

```JSON
{
    "OpenBazaar Contract": {
        "OBCv": "0.1",
        "Category": "Currency",
        "Sub-category,": "Crypto-Crypto",
        "Nonce": "356a192b7913b04c54574d18c28d46e6395427ab",
        "Expiration":"2014-08-29 12:00:00"
        },
    "Seller": {
        "NymID": "61768db8d1a38f1c16d3e6eea812ef423c739068",
        "NodeID": "abc123",
        "BTCuncompressedpubkey":"044448c02963b8f5ba1b8f7019a03b57c80b993313c37b464866efbf61c37098440bcdcc88bedf7f1e9c201e294cf3c064d39e372692a0568c01565b838e06af0b",
        "publicKey": [
            "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",
            "=dDLn"
            ]
        },
    "Currency": {
        "CurrencyPair": "LTC/BTC",
        "ExchangeRate": "0.01637",
        "Type": "Buy",
        "Size": "5",
        "PaymentAddress": "Lbnu1x4UfToiiFGU8MvPrLpj2GSrtUrxFH"
        }
}
```

A prospective buyer only needs to submit a *bid offer* by appending the following data to the *seed contract*:

1. Buyer details
2. Payment address

As with all Ricardian contracts on *OpenBazaar*, digital signatures are required at each step. 

### 2. Crypto-Fiat Currency Exchanges
For a crypto-fiat exchange, the *seed contract* will have the following data fields:

1. Crypto-Fiat currency pair (e.g. Bitcoin/US dollar; BTC/USD)
2. Exchange rate for the currency pair (e.g. 0.00167)
3. Type (i.e buy, sell)
4. Size (i.e. amount of the currency to buy/sell)
5. Payment address/bank details (this data field can be blinded until funds are transferred to the bitcoin multisignature escrow address)

For example, Alice desires to purchase 5 bitcoin for a price of 0.00167 BTC/USD ($3000 USD total). She creates the following *seed contract* and broadcasts it on *OpenBazaar*:

```JSON
{
    "OpenBazaar Contract": {
        "OBCv": "0.1",
        "Category": "Currency",
        "Sub-category,": "Crypto-Fiat",
        "Nonce": "356a192b7913b04c54574d18cd8d46e6395427ab",
        "Expiration":"2014-08-29 12:00:00"
        },
    "Seller": {
        "NymID": "61768db8d1a38f1c16d3e6eea812ef423c739068",
        "NodeID": "abc123",
        "BTCuncompressedpubkey":"044448c02963b8f5ba1b8f7019a03b57c80b993313c37b464866efbf61c37098440bcdcc88bedf7f1e9c201e294cf3c064d39e372692a0568c01565b838e06af0b",
        "publicKey": [
            "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",
            "=dDLn"
            ]
        },
    "Currency": {
        "CurrencyPair": "USD/BTC",
        "ExchangeRate": "0.00167",
        "Type": "Buy",
        "Size": "5",
        "PaymentAddress": "1HZwkjkeaoZfTSaJxDw6aKkxp45agDiEzN"
        }
}
```

On the other side of the trade, the buyer (Bob) submits a *bid offer* by appending the following data to the *seed contract*:

1. Buyer details
2. Bank details (blinded)
  - The bank details can be hashed and included in a single data field
  - Later the source bank details can be revealed to the buyer and arbiter, who can verify the authenticity of the hash

The contract with the buyer's digital signature is sent back to the seller (Alice), which can be signed and forwarded to the arbiter for creation of the multisignature escrow address. Bob transfers 5 BTC to the multisignature address and reveals his bank details to Alice and the arbiter. Bob then awaits for the fiat funds to arrive in his account before signing a release of the funds from the multisig address to Alice.

#### Chargeback Risk

If an irreversible payment processor (e.g. OKPay) is used, then the multisignature bitcoin address is a sufficient means of managing the exchange risk as chargebacks are theoretically impossible. However, if Alice in this case did not use such an non-reverisble payment processor, the trade can still occur with the punative measures to appropriately manage the charageback risk:

1. Probation
  - The purchased bitcoin funds are kept within the multisignature address until the chargeback risk duration has elapsed. This period will be variable, depending on the bank that is used.
2. Surety bond
  - The seller can post a surety bond (refundable security deposit of bitcoin within multisignature address) for the partial or full amount of funds to be exchanged. This allows the seller to access the exchanged bitcoin immediately. While this appears to be a zero-sum game for a single trade, a regular trader may set aside a pool of funds to be re-used as a surety bond after every trade.

### 3. Fiat-Fiat Currency Exchanges
For a fiat-fiat exchange, the *seed contract* will need the following data fields:

1. Fiat-fiat currency pair (e.g. Euro/US dollar; EUR/USD)
2. Exchange rate for the currency pair (e.g. $1.36 EUR/USD)
3. Type (i.e buy, sell)
4. Size (i.e. amount of the currency to buy/sell)
5. Bank details (this data field can be blinded until funds are transferred to the bitcoin multisignature escrow address)

For example, Alice desires to purchase $100 Euro for a price of $1.36 EUR/USD ($136.48 USD total). She creates the following *seed contract* and broadcasts it on *OpenBazaar*:

```JSON
{
    "OpenBazaar Contract": {
        "OBCv": "0.1",
        "Category": "Currency",
        "Sub-category,": "Crypto-Fiat",
        "Nonce": "356a192b7913b04c54574d18cd8d46e6395427ab",
        "Expiration":"2014-08-29 12:00:00"
        },
    "Seller": {
        "NymID": "61768db8d1a38f1c16d3e6eea812ef423c739068",
        "NodeID": "abc123",
        "BTCuncompressedpubkey":"044448c02963b8f5ba1b8f7019a03b57c80b993313c37b464866efbf61c37098440bcdcc88bedf7f1e9c201e294cf3c064d39e372692a0568c01565b838e06af0b",
        "publicKey": [
            "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",
            "=dDLn"
            ]
        },
    "Currency": {
        "CurrencyPair": "EUR/USD",
        "ExchangeRate": "$1.36",
        "Type": "Buy",
        "Size": "100",
        "BankDetails": "8c2226df637baf568e26c042dc376f5d4a1492e8"
        }
}
```

The buyer, Bob, submits a *bid offer* for the *seed contract* by appending:

1. Buyer details
2. Bank details (blinded)

The double-signed contract is sent to the arbiter for the creation of the multisignature escrow address. Both the buyer and seller transfer bitcoin to the address as collateral for their fiat exchange.

As for chargeback risk, it is clearly preferable for both parties to use a non-reversible payment processor. In the absence of this, the bitcoin collateral would not be released until the chargeback risk period as elapsed. 

### Listing and Matching Buy/Sell Orders on *OpenBazaar*

#### 1. DHT Listing
Following the [Bitsquare model](http://Bitsquare.io), the buy and sell orders are listed in the DHT (for a fee, which I'd like to avoid). The client then matches the buy and sell orders and places both parties in contact with each other over the network. Contracts can be found using the inverse keyword search on *OpenBazaar's* DHT, which is a Kademlia-like P2P network.

#### 2. DarkPool
This model uses a private means of matching buy and sell orders, which I think also encompasses private P2P exchanges between parties that have already discovered each other. In this case, the transfer is fairly straightforward and would proceed as I described under the crypto-crypto section.

#### 3. Exchange Nodes
This introduces the concept of a node or groups of nodes where buyers and sellers submit their orders to (Ricardian contracts as described in the article). The exchange matches the buy and sell orders, and is the arbiter for transactions. It will also have responsibility for broadcasting the price on their exchange. So this takes advantage of the benefits of centralisation, but if the exchange is taken out it doesn't cripple the ability for individuals to make currency exchange.

## 4.6 Insurance

Insurance is a valuable service that is often misinterpreted, over-regulated, and out of reach for many people. On *OpenBazaar*, insurance is contextualized as a service that can be offered by anyone or group, leveraging: 

1. The clarity of terms and conditions set out in a Ricardian contract
2. An open marketplace for competing insurance policies and service providers
3. The transparency that crypto-currencies provide via the blockchain

Ultimately, the market will decide the most appropriate insurance service and contract models for *OpenBazaar*. In this article, we propose some potential implementations as a primer for future development. 

### 1. Insurance Services 

Insurance services fundamentally offer: 

1. A pool of funds to offset a cost that exceeds the capacity of the client to pay within a timely manner
2. A pool of funds to compensate another party that has been wronged by actions of the client, which addresses their capacity to pay within a reasonable time frame
3. A dispute resolution organization that represents the interests of the client

These services are of course provided through the lens of *risk assessment* to dictate the price of: 

1. The insurance premium (regular cost of the policy)
2. The excess (fee paid when insurance is triggered). 

Traditional business models of insurance mirror a fractional reserve system in that funds are predominantly reinvested on the statistical reality that it is unlikely for all or even most policies to be triggered at once. Whether this model survives the blockchain era is unknown. The transparency that the blockchain provides may alter consumer preferences for how insurance funds are managed; *proof of solvency* may be an inescapable market-imposed regulation. If so, insurance providers will need to prove that the proportion of funds kept in reserve and those reinvested are at the levels that the policy claims. Alternatively,  conservative insurance providers may keep 100% reserves and seek to raise profits only by fees incorporated into the premium and excess. 

Whatever business model an insurance provider adopts, the terms and conditions of their policy will be listed within a Ricardian contract in *OpenBazaar*, with digital signatures as the unmistakable evidence of agreement.

While the various fields necessary for a comprehensive contract are difficult to predict, the *seed contract* may contain the following core data fields:

```JSON
{
    "OpenBazaar Contract": {
        "OBCv": "0.1",
        "Category": "Insurance",
        "Sub-category,": "Car-Insurnace",
        "Nonce": "01",
        "Expiration": "2014-06-29 12:00:00"
    },
    "Seller": {
        "NymID": "Samuel Patterson",
        "NodeID": "SamPatt",
        "BTCuncompressedpubkey": "044448c02963b8f5ba1b8f7019a03b57c80b993313c37b464866efbf61c37098440bcdcc88bedf7f1e9c201e294cf3c064d39e372692a0568c01565b838e06af0b",
        "PGPpublicKey": [
            "mQENBFMKYTUBCADD6kIAjBlJ2Q0NEd97aia0BSBibO1C2lVemWig5cNATeob1McWoEo3QznZ",
            "f9LaRsuo+ryyqEeXx4p9m9FG/TDeeZvOaiSo2Pg5MtgAdxwJK3ZQ+6b6DjrfYRZplD4qVsQd",
            "/GhxDr733NBdTpfvE8rYrAttNeU8P9vZpJuU+ESSRNb8Sbgfrym2i7xeMP6/xnyfTunXti7x",
            "sJzFkGIoF1dc+HntP2b9oSy1y0P5n0FdGt7IaEEtN1AZwAfFJ0obSlXiVdWdPwyambAzj93V",
            "XD//KxXyxAHjqFD+sdqDzqxBTZ/MC/YCMU1UQ5hvWsx7FRqubLhyMB1p1ycvdnX6nqdTABEB",
            "AAG0KFNhbSBQYXR0ZXJzb24gPHNhbUBzYW11ZWxycGF0dGVyc29uLmNvbT6JATkEEwECACMF",
            "AlMKYTUCGwMHCwkIBwMCAQYVCAIJCgsEFgIDAQIeAQIXgAAKCRCI0UoG/7KgjtsoCACaiEO1",
            "0u22dGDSuBLvFV2UIhyh2Cj9+KEGe7qHlohwbFHqwT+/6B7P5CmBggqL0abMqEqkUCOxk7g4",
            "rBdCSMeqGe+7VdsXVnNAlEYGd6AqQxfcIPV/r/E0qCUQcJbBxjO8rr7il2o4b98AeU8GB6qU",
            "8YFblsCv0KlFRYhDngHVfRMpvsTm9NMcm99P6n/UVn2mrhk50EqJMHB7T+b4n0fWQj5M9340",
            "lmVWnn1i1w3+yDC0v4yLS9UuSuL3UsnC6CP7XPA2z5iVqoTH+i0WguGgGgyUpPIQexGm/IKx",
            "b5Bb/rRsKpcXtdIT7Vr5US4XtCBe9zWvSWyEAz3llYA/OGx+uQENBFMKYTUBCACToWiWhjWI",
            "aJM1pD0TXTk/IbNWZPhJpfAiYy3EsYwckvAY1Q6yOSWydakimpr+93oBpJpY9y6Aj8GDBNyv",
            "eaAomK2jYV0cqxhOzAH2UgSLptFQawxwR/8+rLz5KDZQGT8SNNDwV7FtFT4ubzeH+ILugnJp",
            "Jo83IjoU5Vg3g1d75Tc38KLd8VbwZ52QcxNaoxoEHQoF8Q+2rRNPwRRr5NX71jQPA6Pxuzpd",
            "mTogQ5gIA3Zr4cDrxAAPCZAo/bDJF3jhd9lpZXFudlyCwbRNF3qfG9L0S7ES+zIfXFWmryQb",
            "CUg4pcRfbaN9OY6GWzjOLOzXzEehd7sNbNCKEP3tWqbjABEBAAGJAR8EGAECAAkFAlMKYTUC",
            "GwwACgkQiNFKBv+yoI4kHQgAiz6mQz0GeOfB60ILe1R/9fkYtejFTTb5P8H/w19S2WM+gFyb",
            "R4BTg0OSN0YhcmzIzgy1zF1/J+noxEVUS5zbGLytVgLFECyOinFZUc0A/b0nuermvKFuF7GJ",
            "QcV3Gz1HCNrfaWK7LW5hGTdQZtaFtEmla6Wk+v1MHWWW2K9Ez0KXCqrK5b0Oji2MUh8LRh2p",
            "mmciki/R0pXXMYMJpeb9IPwOlbIqm8xT4Z93rPYyOwmgii1Dy3Gop/Ofq4yU7GGxjjALR+JM",
            "yebij7AblNTCSgWXm/QV+ejZH0bR91FbtjdVKoFo6qrL04zuRbBFm2wHv+CYNKwRR2PPcCB/",
            "dQaGpQ==",
            "=IB1U"
        ]
    },
  "Insurance": {
    "PolicyNonce": "abc123",
    "Term": "12 months",
    "Type": "Car",
    "PolicyValue": "20 BTC",
    "Reserve": "20 percent",
    "Coverage": {
      "Fire": "True",
      "Water": "True",
      "Flood": "False",
      "AccidentalCrash": {
        "IFCauseClient": "False",
        "IFCauseNotClient": "True"
          },
      }
    "Client": {
      "Payments": "0.1 BTC"
      "PaymentFreq": "Monthly"
      "Excess": "0.25 BTC"
      },
    }
}
```

When a client takes out an *insurance contract* with a provider, a multisignature transaction is created in combination with the notary's pubkey. The client's insurance funds are paid to the multisignature transaction according to the payment schedule specified within the *insurance contract*. Under the client and/or notary's supervision, the insurance provider cannot extract funds beyond the reserve amount stated in the contract. For large insurance policies, a notary pool can be used for redundancy's sake and to minimize risk for client and insurance provider. Before an *insurance contract* is signed by the client and insurance provider, the latter can provide evidence that they have sufficient funds to payout the full value of the policy (e.g. 20 BTC in the case of the *insurance contract* above).

In the event that the client makes an insurance claim, the client can provide the relevant detail and evidence of the claim to the insurance provider. For a successful claim, the insurer can transfer the required amount directly to the client. If the insurance provider rejects the claim, the client may flag a dispute.

### 2. Transaction Insurance on *OpenBazaar*

It is possible for users to take out insurance for trades made over *OpenBazaar*. The purpose of the insurance in this context is to guard against the scenario where a user is subject to a loss of funds and/or goods that **cannot** be recovered. This loss is not necessarily due to fraudulent activity, but also covers the accidental loss of funds/goods in transit (i.e. including acts of God).

For example, Alice sells a cat to Bob on *OpenBazaar*:
1. During transit, the cat escapes and an empty cage is delivered to Bob. Bob refuses to sign a multisignature transaction releasing funds to Alice based on the a perceived violation of the contract. 
2. Alice and Bob flag a dispute and engage an arbiter to weigh up the case in order to determine who should receive the funds. 
3. During the dispute resolution process, Alice provides sufficient evidence that the cat was delivered alive and well to the courier. Likewise, Bob is able to provide evidence that the cage was delivered empty to him by the courier.

If the courier in this example *did not* have a presence on *OpenBazaar* (i.e. outside of its jurisdiction), the arbiter may have no choice but to rule in favor of Bob (according to their arbitration policy). The arbiter instructs the notary (the third signature of the 2-of-3 multisignature escrow address between Alice and Bob) to create and sign a transaction releasing the funds back to Bob.

In this case, Alice was not at fault but ended up suffering the consequences of an uncommon accident, which is an ideal scenario for insurance to cover. If Alice had taken *merchant insurance* for her trade with Bob, she could make a claim to cover the value of the lost good in the failed trade. The insurance provider may launch their own investigation before deciding to award Alice the insurance funds.

If the courier in this example *did* have a presence on *OpenBazaar*, Alice's insurance provider may contact the courier directly or the courier's insurance provider. In the latter case, compensation to Alice can be paid out of the courier's insurance policy funds (most likely after arbitration between both insurance providers).

## 4.7 Crowd Funding

_Pending_

## 4.8 Prediction Markets

_Pending_