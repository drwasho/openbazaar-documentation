# 9. Frequently Asked Questions

## General

### Does _OpenBazaar_ cost anything?

No... and it never will.

### Does it cost anything to list items on the network?

No... and it never will.

### Do I need to be online to sell items?

At this stage, yes. When you create a contract for an item, you need to keep your node online for others to be able to download your contracts and place order.

In the future, contracts will be embedded into the distributed hash table (DHT) and shared by nearby nodes on the network over a period of a few days. This will create a temporary cache of contracts that can be accessed without the merchant needing to be online. In addition, orders will be encrypted and embedded into the DHT and shared by nodes so that the merchant can access orders even if the buyer isn't online.

### Can I get a notification when my item has received an order, or when my order has been processed?

No, but this is an important feature we will be working on in the future.

### Is there a mobile application for _OpenBazaar_?

Not yet.

### Will I need to run a node to browse or buy things from _OpenBazaar_?

There are some emerging third party services that will act as a search engine for browsing listings on _OpenBazaar_, such as [BazaarBay](https://bazaarbay.org).

You will need to run a node to buy items from users on _OpenBazaar_ at this stage. You can use services such as [Provistor](https://provistor.com) that faciliate running a node.

---

## Payments without Bitcoin

### Can I buy and sell things with altcoins instead of Bitcoin?

Yes, but using a third party exchange such as [Shapeshift](https://shapeshift.io). Altcoin support is not built into the client.

### Can I buy and sell things with fiat currency?

Yes, but using a third party exchange like [Coinbase](https://coinbase.com), [Circle](https://circle.com), [CoinJar](https://coinjar.com) etc.

### Why only Bitcoin?

We believe in Bitcoin. While there may be several other crypto-commodities created with interesting properties, only one will remain the 'backend' medium-of-exchange: Bitcoin.

---

## Illicit Items

### Is this a new Silk Road?

Absolutely not. The Silk Road was a centralized platform, controlled directly by a small group who ran it for profit. The marketplace was built specifically for a small subset of trade: illicit goods. 

_OpenBazaar_ is entirely different. It’s a decentralized platform, not controlled by anyone, and not run for profit. It’s not being built for any subset of trade, but gives anyone in the world the power to buy and sell any type of goods and services with anyone else, using Bitcoin.

_OpenBazaar_ isn’t Silk Road - version 2, it’s E-Commerce - version 2.

### How will you keep illicit goods off of _OpenBazaar_?

_OpenBazaar_ is a protocol for trading goods online, and a decentralized network for people to communicate with each other. Since the network isn’t centrally controlled (by us or anyone), we cannot directly prevent people from selling illicit goods.

The platform uses a reputation and rating system, as well as allowing reputable third parties help manage trade. We expect these checks and balances will strongly encourage legitimate trade on the network.

Sellers on the _OpenBazaar_ network host their own products and are therefore directly responsible for complying with local laws (and their own conscience) when listing items or services. Users engaged in illicit activity cannot hide behind a third party service.

The world doesn’t have a standard protocol for trading goods and services online, nor does it have a network to share information about those goods and services. _OpenBazaar_ finally creates a protocol and network that is free and open for all. As such, we expect that the use of _OpenBazaar_ will largely reflect how society trades at large - almost entirely legitimate, positive trade with a small portion of people using it for illicit activity. Those that attempt to sell illicit material on __OpenBazaar__ do so at significant risk to themselves.

### What about _OpenBazaar_’s beginnings as Dark Market?

Dark Market was a winning entry in a Toronto hackathon in April 2014. Following the release of the source code on Github, the developers of Dark Market decided to discontinue the project. A few days later, Brian Hoffman forked the project to continue development. Brian renamed the project to _OpenBazaar_, which reflected a shift in vision to create a free and open marketplace for all goods and services.

Since April 2014, we’ve seen _OpenBazaar_ change substantially from the original Dark Market project. Only a small amount of the original code remains, and the majority of the features have been built by the _OpenBazaar_ community, including the use of Ricardian Contracts, HD signing keys, and kademlia DHT.
