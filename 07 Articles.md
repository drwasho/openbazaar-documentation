# 7. Articles

Index:

1. OpenBazaar Threat Model
2. OpenBazaar and Subspace
3. The Onion Shipping Protocol

## 7.1 OpenBazaar Threat Model

This article outlines the security model and policies of OpenBazaar. It is written for contributors with push access who are asked to review and merge pull requests, external developers who are interested in submitting pull requests, and end-users who are interested in understanding the security model of OpenBazaar. We will also be including this threat model in our project documentation, but we are sharing on our blog first to solicit feedback from our users and security experts in the field.

Every contributor with push access must read this document.

### Threat model

OpenBazaar makes important assumptions about the strength of its adversaries. To understand how to properly develop code for OpenBazaar, it is crucial to understand who the adversaries of OpenBazaar can be, what resources they are able to employ, and what their goals are. Furthermore, it is important to understand what the adversaries are not capable of.

#### Assumed adversaries

Our adversaries can be broadly categorized as 4 different entities:

1. Malicious users
2. Malicious corporations
3. Malicious governments
4. Malicious developers

Each of these constitutes a separate entity with different resources and different goals. These are explained below.

#### Malicious users

A malicious user is a user who tries to break the security of OpenBazaar, usually for financial gain. We treat malicious users as game-theoretic agents who are able to invest approximately as much as they would win out of a security breach, as long as their winnings are significantly larger than their losses.

The goal of malicious users is to make money. The two primary ways of making money by breaking the security of OpenBazaar are these:

+ Being able to receive a product without making a proper payment
+ Being able to receive money without delivering a product

As these attacks are financially incentivized, there is no limit as to what capital can be invested in such attacks if it’s possible to earn it back. However, for our purposes, we assume that such attackers are limited to up-front investments of $1,000,000 per year collectively. Thus, they are not able to, for example, break the bitcoin network security, or attack 1024-bit RSA keys.

These attackers are the easiest to model, as they play within the closed OpenBazaar system and can be treated game-theoretically. Generally, in our games, these agents can be assumed to be ε-good, meaning they will not attempt a malicious strategy if there is no financial gain in it. We aim to fully protect users from such malicious actors.

#### Malicious corporations

Certain corporations may find the OpenBazaar network undesirable and may want to break its security in order to bring it down. Their financial incentive may not be part of the closed OpenBazaar system: They may be able to make profits outside of OpenBazaar by making OpenBazaar unreliable and insecure.

The goals of such agents are the following:

+ Bringing down the majority of OpenBazaar nodes
+ Disrupting the majority of connectivity of the OpenBazaar network
+ Breaking the trust people have on the OpenBazaar network

“Breaking the trust” here means creating arbitrary buyers and sellers that do not follow the expected strategy and default on their payments or shipping.

The incentive for such corporations may be that they are losing money because of competitive sales on the OpenBazaar network.

We currently assume that such corporations are able to spend similar monetary amounts as malicious users to attack the network.

However, malicious corporations cannot be modeled as ε-good, as they wish to cause harm on the network through external incentives. We aim to partially protect our users from such malicious actors, through reputation systems and positive margins in our nash equilibria that can decentivize such malicious actors. Reputation systems which require proof-of-burn or similar sybil-resistant schemes help in making these attacks more costly. Webs-of-trust can fully protect careful users from such malicious actors, although great care is required from the user side.

#### Malicious governments

As the OpenBazaar software can be operated worldwide, malicious governments should be taken into account. Malicious governments may wish to bring the network down for censorship reasons or for legal reasons.

The goals of such agents are similar to the goals of malicious corporations. In addition, a malicious government has the following goals:

+ Unmask the anonymity of an OpenBazaar user
+ Block certain categories of products or individual products from being traded

A malicious government has similar resources as a malicious corporation.

Malicious governments can be categorized into active and passive based on their willingness to interfere with networks. A passive government is unwilling to manipulate data as a man-in-the-middle at the network level, and is only willing to be a passive eavesdropper. An active government is happy to interfere with network traffic by manipulating it.

A passive government has access to these additional resources:

+ They can manipulate the legal framework of their country
+ They can introduce new laws
+ They can issue arbitrary subpoenas and warrants
+ They can issue secret warrants and take decisions in secret courts

An active government has, in addition, access to the following resources:

+ They can sybil-attack the issue of identification documents such as passports
+ They can man-in-the-middle Internet connections within their country (up to the Tor security assumptions)
+ They can break Internet connectivity within their country
+ They can issue arbitrary PKI certificates for HTTPS and TLS protocols

Malicious governments are the hardest attack to guard against. While our goal is to be able to defend against malicious governments, we are not able to do this currently. Our decentralization efforts are oriented around the idea that basic protection from malicious governments should be possible.

We aim to provide full protection against a passive malicious government, and partial protection against active malicious governments.

On the topic of denial-of-service attacks by breaking network connectivity completely, we do not have a mechanism of defence. We rely on the fact that countries will prefer to keep Internet connectivity for the most part of their users. Mesh networks can be used to guard against such attacks, but these are beyond the scope of our threat model.

#### Malicious developers

The last malicious actor is a malicious developer with commit access. These developers are manipulated by one of the above actors through law (secret subpoenas) or bribery in order to achieve their end-goals. Therefore, the goals of a malicious developer are aligned with the goals of those above.

We prefer to list this malicious actor separately, as they have access to a different arsenal of attacks.

In particular, a malicious developer has access to the following resources:

+ They can merge arbitrary pull requests, in effect writing arbitrary code

Our primary means of defence against malicious developers are secure development practices, some of which we explain later in this document.

#### Security assumptions

We rely on the assumption that certain systems are secure. We make the assumption that the following constituent parts of our system are secure:

+ RSA, ECDSA, SHA256, AES and the rest of the cryptographic primitives used by the software
+ GPG
+ Bitcoin
+ Namecoin
+ Tor (including its assumptions on network monitoring)
+ The browser used by the user
+ The user’s computer
+ Our implementation languages and frameworks (Python, Javascript, Angular)
+ The libraries we rely on which are listed here: https://github.com/OpenBazaar/OpenBazaar/blob/master/requirements.txt

#### Beta status

OpenBazaar is currently in beta and is [facing multiple security issues](https://github.com/OpenBazaar/OpenBazaar/issues?q=is%3Aopen+is%3Aissue+label%3ASecurity), in addition to the assumptions above. These issues can cause your transactions to be compromised, your money not to go through or be stolen, or your products not to be delivered, even by attackers with very limited resources. We estimate that an attacker could possibly compromise the system’s security for less than a few thousand dollars. Please be careful when using OpenBazaar at this stage. If you’re using the regular bitcoin network, **keep the amounts exchanged low** (less than a few hundred dollars). Don’t sell your car on OpenBazaar yet!

For the beta version, we rely on the security of HTTPS and PKI. This is needed, as we rely on pip for securely fetching python packages, which relies on HTTPS. Therefore, an active malicious government is able to interfere with OpenBazaar. The specific assumptions are listed below.

However, we are interested in dropping this requirement for stable versions. We are still unsure of the distribution channels to achieve this.

As part of our beta status, **we also rely on a few centralized pieces of infrastructure**, all of which we would like to replace with decentralized systems. These are our assumptions for the beta version:

+ PKI is secure. CAs are not compromised or government-controlled.
+ HTTPS is secure.
+ The operators of seed servers are not interested in performing denial-of-service attacks.
+ Obelisk and the official obelisk servers are secure and their owners not malicious.
+ The official DNSChain servers are secure and their owners not malicious.

The last two requirements are strong. A user who is concerned with these last points can lift these requirements by running their own Obelisk server or using a trusted Obelisk server, which requires maintaining a bitcoin blockchain copy, and their own DNSChain server, which requires maintaining a namecoin blockchain copy. We have plans to use SVP systems as a replacement for these.

We may introduce additional points of centralization for future beta versions in order to create a working product, which we plan to lift later.

As part of our Beta status, we do not currently employ any anonymity-preserving mechanisms. All transactions are completely trackable. Therefore, malicious governments with the goal of uncovering your identity will succeed trivially.

#### Development infrastructure

We also rely on certain pieces of centralized infrastructure for development. Here are our assumptions about services we use for development and we assume are secure and with no malicious operators:

+ GitHub
+ Slack
+ Gmail

We also rely on OTR and GPG for secure development-related communications.

#### Development process

In order to guard against actors such as malicious developers, we are publishing all of the source code of OpenBazaar as open source under the [MIT license](https://github.com/OpenBazaar/OpenBazaar/blob/master/LICENSE.md). We encourage security experts to audit our code for security vulnerabilities. Don’t take our word on the security of OpenBazaar; inspect the code yourself. **Don’t trust the OpenBazaar developers**. While we’re doing our best to provide secure software, the reason it’s open source is so that it can be reviewed and audited.

Our development process is transparent and security-oriented. We follow certain contributing guidelines which require code reviews and follow proper release cycles. The details of our contributing procedures can be found in the [CONTRIBUTING](https://github.com/OpenBazaar/OpenBazaar/blob/master/CONTRIBUTING.md) document. We don’t just make our code open source. Our reviews and commit history are also available for inspection, so that differential audits are possible.

We GPG-sign our releases with contributor keys in the strong GPG set. Currently, Dionysis Zindros signs releases. The signature signifies that the release maintainer believes the source code to be secure at the time of release, and that it was intentionally released by the team, without backdoors or intentional vulnerabilities as far as we know. These signatures are hard to fake by someone who wishes to introduce backdoors in the future. If you require strong security, do not run releases that have not been GPG-signed, or manually inspect the code. Otherwise, at least make sure your binaries and sources are at least downloaded via HTTPS.

Our decision-making procedure is transparent too. We use 2-of-4 multisig decision-making for financial decisions. The details of our transparency in decision making can be found in our [transparency post](https://blog.openbazaar.org/migration-of-our-project-funds-to-a-multisig-address/).

Some of our commits are GPG-signed. The GPG-signed commits signify that:

+ The code introduced in the commit is believed to be secure by the signer
+ The parent of the commit is the currently available branch to the signer

Please note that signing a commit, contrary to popular belief, does not indicate that all the previous commits have been checked by the committer. It only indicates that the particular commit code really is written by the person who signed the commit, and this is it.

It is very easy to introduce fake commits on git and GitHub, using usernames and e-mails of existing contributors. Therefore, we prefer signing individual commits instead of tagging and signing only occassionally to show a full good history. It’s impractical to create a tag for every commit one creates.

## 7.2 OpenBazaar and Subspace

Key features of the Subspace protocol are (as per the readme):

1. All messages are encrypted with only the sender and recipient able to decrypt.
2. The senderâ€™s identity is not attached to the message at all and the only a user-defined number of bits from the recipient address is attached to the message. In other words, no meaningful data is available to passive observers.
3. By splitting the message load among servers (possibly with a financial incentive), the network can scale to handle a very large number of messages.
4. Because the network is server based, servers can implement traditional anti-spam measures without harming user experience.
5. The protocol supports lightweight queries allowing the user to make the anonymity set/bandwidth tradeoff.
6. The protocol supports market based rationing mechanisms where necessary.

This protocol was developed by the ever talented Chris Pacia, from [Bitcoin Authenticator](https://www.bitcoinauthenticator.org/), who significantly impressed several of us when he circulated his initial proposalâ€¦ but then shocked us with how quickly he implemented his proposal in just a few weeks. I think thereâ€™s a lot to be said about this model of development: lean, fast, and with a singular focus on making something.

Currently, Subspace supports 3 types of communication: 

1. Instant messaging between two peers
2. Email (only to other Subspace addresses) 
3. Public or private chat rooms

This is just the beginning of what can be developed on top of Subspace, but already a significant number of things can be accomplished with OpenBazaar using these initial features.

### OpenBazaar and Subspace
There are several potential applications for Subspace within OpenBazaar. I will go through a few of them and unpack how they might work

#### 1. Ricardian Contract Hosting - Subspace can be used by merchants to upload OpenBazaar Ricardian contracts for persistent and private accessibility.
Currently, as with most distributed hash table (DHT)-style p2p networks, content can only be retrieved from a node if it is online. The implications of this is that a merchant would need to run their OpenBazaar node 24/7 in order for potential buyers to access, view, and purchase contracts they are selling. This may not be an option for most merchants, and it is an area where Subspace is ideally suited to solve.

A user will create a contract, submit it to Subspace federated server, who will store the contract either gratuitously or for a small fee over a negotiated period of time. The Subspace server will store the contract and associate the contract with the merchantâ€™s OpenBazaar GUID within the Subspace DHT. When a potential buyer wishes to download contracts created by a certain merchant, they can submit a request from any Subspace server using the merchants GUID as a search term. The server will either return the contract directly (if it stored on its HDD) or search the DHT for the Subspace server that does, and retrieve the contract for the user. Moreover, the contract can be encrypted using key that only the contract participants can decrypt (similar to Bitcoin stealth addresses). This will enable contracts to be stored in Subspace, and referenced in the DHT, without any OpenBazaar GUID associated with the contract.

#### 2. Multiparty communication - private communication and contract creation between market participants
A typical exchange within OpenBazaar involves 3 parties: a merchant, buyer, and notary. Within the Ricardian contract schema, the participants can indicate their email address or other communicative ID for contract related discussion. The contract schema can be modified to include a Subspace address and private room key (where other users can query the merchant about the contract if theyâ€™re online). Conveniently, the private room key can be identical to the contract nonce. Once the contract has been signed by all parties (i.e. merchant, buyer, and notary), these parties can chat privately using Subspaceâ€™s email or private chat room service.

Under the current design, the contract creation process begins with the merchant and then ping-pongs between participating nodes. At each stage, the buyer or notary appends relevant information (e.g. ID, tracking number or digital signatures) and passes the contract to the next party until the contract is completely filled in. Alternatively, a Subspace server could act as a central repository of the Ricardian contract, with push access granted to the market participants.

#### 3. Market channels
Using Subspace's chat room feature, specialised chat rooms or â€˜market channelsâ€™ can be created for categories and subcategories of goods and services where merchants can advertise. While the Subspace server network is Federated, each server will have individual leasing policies for storing data. A possible outcome is for Subspace server operators to charge a subscription fee to prospective OpenBazaar merchants to advertise their goods on a market channel they host. Furthermore, OB-specific market channels may be modified to accept â€˜seedâ€™ Ricardian contracts only (i.e. the initial JSON-formatted contract for the item) rather than plain text posts.

![0](http://s25.postimg.org/5q2j5uklr/OB_Subspace_2.jpg)

#### 4. Auction haggling
Auctions in OpenBazaar are expected to be developed later in 2015. While contracts and auctions can be decentrally distributed and managed using the merchantâ€™s node (or a proxy node with high bandwidth capabilities), they can also be managed via Subspace. The advantage of using Subspace over OpenBazaarâ€™s DHT are relative to the user, but it would allow for casual users to upload their contract and automatically execute the bidding process without operating a node during the period of the auction. In other words, merchants can â€˜fire and forgetâ€™ contracts they put up for auction, returning to the end of the auction period to finalise the OB Ricardian contract with the winning bidder.

Bids may be uploaded, stored, and pulled from multiple Subspace servers; however, updating the contract with the latest price based on the bids will be done by a single Subspace server (who theoretically will have better bandwidth and uptime capacity compared to a userâ€™s node). Bids can be submitted via a public chat room modified to only accept JSON-formatted OB contracts (not plain text) to minimize the potential for spam.

### Summary
Subspace has the potential to become a powerful tool to enable market discovery and coordination, while preserving the privacy of individuals purchasing goods and services. Further work is necessary to develop the APIs to enable most of the features listed above, as well as both Subspace and OpenBazaar clients.

## 7.3 The Onion Shipping Protocol

### 1. Introduction
In online marketplaces, personal identifiable information (PII) can be leaked by the buyer exposing their shipping address. For centralised marketplaces, any abuse of this information can be combatted through traditional legal channels. For a pseudonymous marketplace, this may not be possible.

An ideal shipping protocol is required to deliver a good to a buyer without the buyer exposing any PII that can be associated with their pseudonymous identity. Here we propose a multi-hop shipping network that uses encrypted delivery addresses to obfuscate the final destination of the package from the merchant and most of the shipping network.

### 2. Protocol
The protocol conceptually similar to TOR in that encrypted shipping addresses are layered like an onion to only reveal the next delivery address to shipping nodes. 

1. The buyer pulls up mobile app and enters shipping address
2. App determines distance and finds compatible nodes to route shipping through 
3. The buyer pays out shipping fee to each shipping node's escrow address
4. App presents a printable label for the package that includes three QR codes containing encrypted shipping information for each hop just as Tor
5. The merchant prints label and sticks it on the package and first hop will pick up the package within time window from location of choice
6. Shipper A decrypts address from QR code 1 and delivers it to Shipper B within time window
  - Shipper A updates delivery status
7. Shipper B decrypts address from QR code 2 delivers it to Shipper C
  - Shipper B updates delivery status
8. Shipper C decrypts address from QR code 3 and delivers it to it's final destination
  - Notifies the merchant that it has been delivered
  - The buyer confirms delivery and releases funds from multisignature escrow to all shipper nodes

### 3. Weaknesses
1. The last node in the transport chain knows the shipping address (but not the merchant)
2. The item can be tampered with in transit
3. Higher cost of shipping

### 4. Use Cases
While one may think that the primary application of this protocol is for shipping illicit goods, it is in fact to protect the privacy and security of the buyer when purchasing non-illicit items. The purchase of certain items in a non-private manner may allow a merchant to collect metadata on a user, which can be sold to third parties. Moreover, purchasing high priced certain items may make some users a target for theft (post-transaction).
