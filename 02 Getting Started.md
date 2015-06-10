# 2. Getting started

## 2.1 Installation

Future versions of OpenBazaar will be developed using [Electron](https://github.com/atom/electron), which will enable the platform to be easily installed on all platforms.

Below are instructions for the current version (beta 5.0) of OpenBazaar.

### Mac OSX and Ubuntu (using configure.sh script)

1. Install git:
	+ `$ sudo apt-get install git`
	+ Optionally install package `haveged` for faster entropy creation; refer to [this](https://github.com/OpenBazaar/OpenBazaar/wiki/Build-Instructions#ubuntulinux-wont-get-past-generating-pgp-keypair-or-takes-way-too-long-to-generate-key) section if keypair generation takes too long on Ubuntu/Linux.
2. Get the latest release:
	+ `$ git clone https://github.com/OpenBazaar/OpenBazaar.git`
3. Navigate to the OpenBazaar folder
	+ `$ cd OpenBazaar`
4. Run the configuration script:
	+ `$ ./configure.sh`
	+ Your `OpenBazaar` installation should now be ready to go.
5. Run the application
	+ `$ ./openbazaar start`
	+ If the above script executes successfully, you will see a new browser window open automatically. If this doesn't happen after some time, open your web browser and point it to `http://localhost:<PORT>`. You will find the correct `PORT` number in the console output. Of course, in order to run a browser in the GUI you need to have them both installed (e.g. `sudo apt-get install xorg gnome-core gnome-system-tools gnome-app-install firefox`). Otherwise you can try to use SSH tunneling or some other approach.
6. Test the application.
7. To shutdown `OpenBazaar`
	+ `$ ./openbazaar stop`

===

### Ubuntu (possibly other versions of Linux, let us know)

_Please use the instructions above! The following instructions are for advanced users, and are not up to date._

### Native Installation

First at all, we need to create a virtualenv in order to avoid conflictive dependencies from another old/future projects in the same machine:

1. `$ apt-get update`
2. `$ sudo apt-get install python-pip`
3. `$ sudo pip install virtualenv`
4. `$ sudo pip install virtualenvwrapper`
5. `$ mkdir ~/.virtualenvs`
6. Add the following lines in your `~/.zshrc`, `~/.bashrc` or `~/.profile`: 
	+ `export WORKON_HOME=~/.virtualenvs`
	+ `source /usr/local/bin/virtualenvwrapper.sh`
7. `source ~/.zshrc` (the file where you put the lines in the previous step).
8. `mkvirtualenv open_bazaar` or whatever name that you like.
	+ With the virtualenv already active, then install OpenBazaar:
9. `$ apt-get update`
10. `$ apt-get install git python-dev python-pip g++ libjpeg-dev zlib1g-dev sqlite3 libssl-dev`
11. `$ cd /opt`
12. `$ git clone https://github.com/OpenBazaar/OpenBazaar.git` # may need a "sudo" before "git"
13. `$ cd /opt/OpenBazaar`
14. `$ sudo pip install -r requirements.txt`
15. `$ ./openbazaar start` - Runs `OpenBazaar`
16. Open your browser and give: `127.0.0.1:8888`
17. Welcome to `OpenBazaar`!

===

### Vagrant

These instructions download a VirtualBox image (Ubuntu Trusty) and use Vagrant (>=1.3.0) to configure an OpenBazaar node inside the virtual environment. When the node is running, you can navigate to http://localhost:8888 on your local machine to access the client. This setup should take less than 10GB and about an hour. These instructions should include all necessary code for starting OpenBazaar.

1. This example is built on an Ubuntu Trusty host. Doesn't work from inside a virtual machine.
	+ `sudo apt-get update`
	+ `sudo apt-get install virtualbox git vagrant`
2. Clone openbazaar:
	+ `git clone https://github.com/OpenBazaar/OpenBazaar.git`
	+ `cd OpenBazaar`
3. Set up vagrant: (this will take a while!)
	+ `vagrant up`
4. Log into the vagrant instance:
	+ `vagrant ssh`
5. Start the OpenBazaar node:
	+ *Before running for the first time: `cd /vagrant && ./configure.sh`*
	+ **Production:**`cd /vagrant && ./openbazaar start && tail -f logs/production.log`
	+ **Development:**`cd /vagrant && ./openbazaar -d start && tail -f logs/development.log`
6. Now return to your host and open your web browser to:
	+ `http://127.0.0.1:8888`

===

### Additional Notes

#### OSX Users

You can either install natively or use vagrant. If you decide to use vagrant, just follow the instructions above for Linux. If you want to install natively, follow the instructions below.

**Install Dependencies**

1. Install [http://brew.sh](Homebrew)
2. `brew update`
3. `brew upgrade`
3. `brew doctor`
4. `brew install wget gpg`
5. `sudo port install sqlite3` (you don't have macports? you can always do `brew install sqlite3` and manually symlink it, e.g. `ln -s /usr/local/Cellar/sqlite/3.8.5_1/bin/sqlite3 /usr/local/bin/sqlite3`)

**Install PIP and Virtualenv**

1. `sudo easy_install pip`
2. `sudo pip install -U pip` 
3. `sudo pip install virtualenv`
4. `sudo pip install virtualenvwrapper`
5. `mkdir ~/.virtualenvs`
6. Then in our ~/.zshrc, ~/.bashrc or ~/.profile, add the following lines:
	+ `export WORKON_HOME=~/.virtualenvs`
	+ `source /usr/local/bin/virtualenvwrapper.sh`        
7. `source ~/.zshrc`
	+ or where you put the lines above

**Install OpenBazaar**

1. `git clone https://github.com/OpenBazaar/OpenBazaar.git`
2. `cd OpenBazaar`
3. `mkvirtualenv open_bazaar` or whatever name that you like
4. `pip install -r requirements.txt` (see note below if you encounter a CLANG error)
5. `bash openbazaar start` - Runs OpenBazaar
6. Open your browser and go to `127.0.0.1:8888`
7. Start OpenBazaaring!

If you encounter a CLANG error while installing pyzmq or Pillow you can try installing requirements with the following to ignore unused arguments:

```bash
CPPFLAGS=-Qunused-arguments CFLAGS=-Qunused-arguments pip install -r requirements.txt
```

### Tests
1. Install the required dependencies:
	+ `$ pip install -r test_requirements.txt`
	+ `$ npm install -g jshint`
2. Run the tests:
	+ `$ make`
	+ There are a bunch of behaviour tests on `OpenBazaar/features` folder but these are not functional right now.

### Issues 
+ If you are getting a complaint about not having PIL and you're on Ubuntu try:
	+ `sudo apt-get install python-imaging`
+ If you are having problems sort of like [this](https://gist.githubusercontent.com/joshlemer/b1d2a8173889c368a8b1/raw/b64ac4b3db7c62f00469d57412a6ee6c1bf3f0cb/OpenBazaarError) when you try to run `vagrant up`, try these steps:
	+ Open the Virtualbox GUI client, select the OpenBazaar machine and select 'Start'.
	+ If this doesn't start the machine, but instead returns an error like [this one](https://gist.githubusercontent.com/joshlemer/b1d2a8173889c368a8b1/raw/bb0da8ca637e07a17d6905cfcdc9ecbb3781cf16/VirtualBoxError), then you'll want to try following the advice in [this thread](https://github.com/mitchellh/vagrant/issues/2157). 
	+ If you're still having the issue, you'll want to try entering your system BIOS to enable Virtualized Hardware.

### pyelliptic
+ On MacOS, you may have issues with your OpenSSL version. You may get an error similar to the following:
	+ `AttributeError: dlsym(0x7f9d8b4fbf90, PKCS5_PBKDF2_HMAC): symbol not found`
+	This error is related to a [pyelliptic bug](https://github.com/yann2192/pyelliptic/issues/11). 
+ You can overcome this issue by running the `openbazaar start` command as follows:
	+ `DYLD_LIBRARY_PATH=/usr/local/Cellar/openssl/1.0.1f/lib bash openbazaar start`
+ If this doesn't work a simpler way to fix the "symbol not found" issue is to ensure that the Homebrew OpenSSL is correctly linked by running:
	+ `brew link openssl --force`
	+ then you will be able to use the `openbazaar start` normally.

### Generating a PGP Keypair 
#### Ubuntu/Linux: Won't get past 'Generating PGP keypair' or takes way too long to generate key.
+ When you first run OpenBazaar it needs to create a 2048 bit key, this could take a few seconds, but on some Ubuntu systems it takes so long (>30 min) that you'd give up and try again.
+ When you do a ps aux, you see this process stuck doing nothing:
	+ `gpg --status-fd 2 --no-tty --gen-key --batch`
+ The reason is that to generate the PGP key, the system needs entropy. When you read the `entropy_avail` file (`cat /proc/sys/kernel/random/entropy_avail`) you will probably see a very low number. A solution to having more entropy is to install the ***rng-tools***.
	+ `sudo apt-get install rng-tools`
+ If you have issues starting the rng-tools service like this.
	+ `sudo service rng-tools start`
	+ This returns: 
		- `Starting Hardware RNG entropy gatherer daemon: (failed).`
	+ Then you need to set an environment variable:
		1. `sudo bash`
		2. `echo "HRNGDEVICE=/dev/urandom" >> /etc/default/rng-tools`
		3. `exit`
+ Now try once again to start it.
	+ `sudo service rng-tools start`

## 2.2 User guide

### 2.2.1 Overview of the Client

There are six tabs in the OpenBazaar client:

1. Home
2. Messages
3. Orders
4. Notarizations
5. Contracts
6. Settings

#### Settings

![Settings](https://blog.openbazaar.org/wp-content/uploads/2015/04/OBstore-1024x696.png)

In settings you have six sections to manage your client.

1. Store Info
2. Keys
3. Communication
4. Notary
5. Advanced
6. Backup

#### Store Info

In the Store Info section, you have the following options.

##### Store Details

+ **Nickname**. This is the name of your store that everyone will see on the network. You must enter a store name or it will display as “Default.”
+ **Avatar URL**. This allows you to choose a personalized image which is displayed along with your store. These images are externally hosted, so choose a link to an image of your choice. Avatars are optional.
+ **Namecoin id**. If you have a Namecoin id, you can choose to have it displayed in your profile.
+ Bitcoin Receiving Address. It is important you put in a Bitcoin address that you control. This is where funds will be released if the third party notary needs to manually release funds from multisig.
+ **Store Description**. Give a short description of your store here. Only supports text at the moment.

##### Reputation Pledge

This section displays the amount of your reputation pledge, and your proof-of-burn address. Reputation pledges are a way to intentionally burn a small amount of Bitcoin tied to your store ID in order to show others you are committed to maintaining your store reputation. In other words, someone who has made a sizeable reputation pledge is unlikely to be a scammer, since it wouldn’t be profitable for a scammer to consistently burn coins for new identities. You can learn more about reputation pledges [here](https://blog.openbazaar.org/proof-of-burn-and-reputation-pledges/).

Note that during betas we don’t recommend large reputation pledges, since there’s a good chance your store may need to be updated and you will lose the identity associated with the pledge.

To make a pledge, simply send a small amount of Bitcoin to the address listed as “Proof-of-burn address.”

##### Shipping Information

This is where a buyer will input their shipping information. If you intend on using OpenBazaar as a buyer as well as a merchant, you should fill this section out as well.

#### Keys

##### Bitcoin Public Key (Uncompressed)

This is the uncompressed Bitcoin public key created for signing.

##### BIP32 Seed

The OpenBazaar client uses BIP32 to create HD keys for signing. This increases privacy by ensuring that the same key isn’t used for signing multisignature transactions. This seed should be kept private.

##### PGP Public Key

In order to encrypt communications over the network, each store creates a PGP key pair. This is the public key which other users’ clients use to encrypt messages sent to you.

#### Communication

+ **Email**. If you’d like to communicate with other users over email, set it here. Your email will be visible to anyone viewing your store.  
+ **Your Website**. If you’d like to have your website URL displayed in your store, set it here.  
+ **Bitmessage**. If you’d like to communicate with other users over Bitmessage, set it here.  

#### Notary

Notaries are a vital part of OpenBazaar. They are the third key holder in the 2-of-3 multisig, meaning that if there is a dispute between buyer and merchant, only the notary has the power to work with one of the parties to release the funds. As such, it’s important that buyer and merchant trust the notary not to collude with the other party. In beta we recommend smaller transactions until reputable notaries emerge in the market. Since in the 5.0 beta buyers choose notaries, the burden is on the merchant to either accept the buyer’s choice of notary or contact the buyer and notary to tell them you don’t want to engage in trade with the other parties.

##### Trusted Notaries

This is a list of notaries that you’ve trusted. You can also add a notary manually by entering their GUID (the string of numbers and letters under their store name).

##### Notary Details

This section is for notaries to set up their services.

1. **Make me a notary**. By clicking Yes in this section, you allow others to choose you as a notary. They will see this option when they click on your store front and see the “Services” tab, or by manually entering your GUID into the Trusted Notaries section. The default option is set to No; users aren’t notaries unless they choose to be.
2. **Fees**. As a notary, you can charge a percentage fee for providing dispute resolution. If the buyer and seller finish their transaction without needing the notary, there is no payment. If the notary is needed to refund buyer or release payment to merchant, then they will receive the percentage from the multisig that they set in this section. A notaries’ fee is visible in the “Services” tab in their store front.
3. **Description of your services**. Notaries can explain their terms and conditions in this area, as well as their credentials and any other information they wish to share.

#### Advanced

##### Obelisk Server
This allows the user to manually select an Obelisk (libbitcoin) server.

##### Developer Tools
These allow the user to clear their cache, clear the peers stored in the database, and to stop their own node.

##### Log
The log can be used for troubleshooting and bug reports.

#### Backups
You can create new backups with the “Create New Backup” button, and they will be displayed below the Backup Name section.

#### Home
The Home tab displays Other Markets, allows the user to search for products, and serves as a simple place to communicate via the Chat Stream.

##### Other Markets
![Seller](https://blog.openbazaar.org/wp-content/uploads/2015/04/merchantOB.png)

You can view the other stores connected to you by clicking on them. Stores with a checkmark should be visible. Stores with an X were visible once, but are now offline. The client should automatically pull in new stores as they become available, but occasionally refreshing the page may help.

When viewing a store, there are three sections.

1. **Store**. This displays the merchant’s products. Clicking on the image displays more details.
2. **Details**. This displays information about the merchant, including their OB public key, PGP key, amount of their reputation pledge, and any communication information they’ve displayed.
3. **Services**. This is only visible if the user offers notary services. If they do, it will display their percentage fee, a description of their services, and allow users to select them as notaries by clicking “Make Trusted Notary.”

Users can also contact the store owner by selecting the “Message me” or “Email me” buttons in blue underneath their name.

##### Search
When creating an item, merchants tag them with keywords. Buyers can then use the search bar to find items tagged with those keywords. Clicking on an item brings the users to the merchant’s store front.

##### Chat Stream
This is a simple chat that any node can use to communicate with all other nodes it is connected to. Note that this feature isn’t likely to scale well and will be removed in future releases.

##### Purchasing a Product
![Puchasing a product](https://blog.openbazaar.org/wp-content/uploads/2015/04/PipeExampleOB.png)

If you click on an item in a store, a new window opens to give more product details, including the product title, price in Bitcoin, product description, cost of shipping and handling, quantity available, the item’s condition, and up to three photos. There is also a “Raw Contract” button which allows users to view the contract details directly.

![Product purchase 2](https://blog.openbazaar.org/wp-content/uploads/2015/04/orderdetailsOB.png)

Clicking on “Order Details” on the bottom left will bring you to a screen that allows you to purchase the product. You can determine the quantity desired, and attach a comment for the merchant to see along with your order. If you haven’t entered your shipping address in Settings already, a red warning will ask you to do so before proceeding. The price for the product and shipping and handling are displayed again.

At the bottom the user needs to input a Bitcoin Address that they control. This will be used in case of a refund.

Once this section is completed, the user selects “Choose a Notary.” A list of online notaries that the user has trusted is displayed. If the user hasn’t trusted any notaries, or if none of those notaries are online, they must choose another notary in order to continue.

The user then completes the order by selecting “Submit Order.” This sends the order to the notary and merchant.

#### Contracts

![Contracts](https://blog.openbazaar.org/wp-content/uploads/2015/04/mycontracts.png)
The contracts tab is where a merchant manages their products. The merchant can create new contracts, edit existing contracts, or delete them.

##### Add Contract

![Add a contract](http://i.imgur.com/jAtFHXV.gif)

1. Click on the Contract tab.
2. Click Add Contract.
3. Input product details, including a title and description of your product, as well as the price (in Bitcoin), the cost of shipping, how many items are available, and what condition the items are in.
4. Add up to three externally hosted images in the photos section.
5. Make sure you click on the Keyword section next to Photos and input keywords that describe your product. This is how users find your items through the search function. If you don’t add keywords, your items cannot be found via search.
6. Click Save. This publishes your product to the network.

#### Orders

The orders tab keeps track of the activity of buyers and merchants through the “My Sales” and “My Purchases” sections.

##### My Sales

If a merchant has a sale, the details of that sale are listed here. An order number is created, along with the time and date of the purchase and the buyer’s details.

![My Sales](https://blog.openbazaar.org/wp-content/uploads/2015/04/sellerOB.png)

A merchant should take the following steps once they’ve received an order.

1. Click on the order to display details.
2. If someone purchases your product, the item will display “Buyer Paid.” **Please double check** the linked multisig account in the order description to verify; at this point a buyer can mark an item as paid without actually paying.
	+ ![My Sales 2](https://blog.openbazaar.org/wp-content/uploads/2015/04/productOB.png)
3. **Determine if you trust the notary involved**. Since at this point the buyer chooses the notary, if the two parties are colluding, they can cheat you out of the Bitcoin. You can view the notary involved by clicking “Contract Details” in the item description. Early in the beta, we recommend test transactions or small transactions until trusted notaries become established.
4. If you verify the buyer has sent the funds to multisig, and that you trust the notary, then ship the item to the buyer at the address they provided. This address is displayed in the “Shipping Information” tab when viewing the order.
5. Once you’ve shipped the item, input your Bitcoin address into the Shipping & Payment section of the order view, where it asks “Where would you like payment sent to?”
6. Once the buyer receives the item, they should release payment. If they don’t in a reasonable time, contact the buyer and request they release funds. If they are non-responsive, contact the notary involved in the transaction and request they release funds.

###### My Purchases

When a buyer views “My Purchases” it will display the status of their orders. If they’ve just submitted an order, the status will indicate “Need to Pay” and the buyer needs to open the order to complete payment.

![My Purchases](https://blog.openbazaar.org/wp-content/uploads/2015/04/needtopayOB.png)

A QR code is displayed which, if scanned, will input the multisignature address and amount. If the user selects “Pay in your Wallet,” it will open a wallet on their device and pull in the same information. Once the payment is completed, the buyer must manually select “Mark as Paid.” This lets the merchant know to ship the item.

If the buyer marked the order as paid, but the merchant didn’t receive this message due to being offline, the buyer can re-open the order and click on “Resend Payment Notice” when the merchant is online.

Once the item has arrived or service is provided, the buyer can then release the funds from multisig by opening the order and selecting “Release Payment to Merchant.” Again, if the merchant didn’t receive this message due to being offline, the buyer can try releasing again when they are online.

#### Notarizations

Notaries manage their orders through the notarizations tab. This is the same as the My Purchases and My Sales tabs, except it tracks the contracts which the notary has been selected for.

Note that at this point, offering notary services means you automatically accept all transactions which choose you as a notary. In the future, notaries will be able to screen transactions, or only accept them manually.

If a buyer or seller contacts a notary asking for funds to be released, it’s the notary’s responsibility to do their best to determine which party should receive funds. Once they’ve made their decision and contacted the parties, they can release funds by opening up the order in the notarizations tab.

![Notarlizations](https://blog.openbazaar.org/wp-content/uploads/2015/04/OBnotary.png)

In the 5.0 beta client, the notary has two options. “Refund the Buyer” releases all the funds from multisig to the buyer, minus the percentage fee which is paid to the notary for dispute resolution. “Release money to the Merchant” does the same for the merchant. The notary must click “Send Resolution” for the transaction to process.

#### Messages

![Messages](https://blog.openbazaar.org/wp-content/uploads/2015/04/OBmessage.png)

The messages tab is a place to communicate with other OpenBazaar users who are online. You can send simple messages (text only at this point) by clicking the “Send a Message” button and selecting another user from the dropdown list. Messages you’ve received can be read by clicking on them, and replied to by hitting the blue “Reply” button on the right.

#### Terminal commands

For Linux and OSX users, you need to use the terminal to configure, start, and stop OpenBazaar. Here are some common commands to use.

1. `./configure.sh` This installs OpenBazaar once the code has been downloaded. After major releases, you may need to run configure again.
2. `./openbazaar help` This gives you a list of arguments you can use when launching OpenBazaar.
3. `./openbazaar start` This launches OpenBazaar.
4. `./openbazaar stop` This shuts down OpenBazaar.
5. `git pull` If git is correctly installed, this will update the software if there are new changes.

#### Tips and Tricks

1. Try refreshing the page occasionally if things aren’t working correctly.
2. Wait a minute or two when first connecting to find peers. It shouldn’t take any longer than this.
3. If you have connectivity problems, try using `killall python -9` in terminal, then launch OpenBazaar again.
4. If you receive a “Address already in use” error when starting OpenBazaar, this means the program was already running. Stop it first, then launch again.
5. If your client crashes or has an obvious error, try looking for /logs/production.log and searching for ‘Traceback’ to see what the error was. If you don’t see anyone else posting about that error on the [Github issues](https://github.com/OpenBazaar/OpenBazaar/issues) and our [Help Desk](https://openbazaar.zendesk.com/hc/en-ushttps://openbazaar.zendesk.com/hc/en-us) then feel free to post along with the error and some context.

### 2.2.2 Merchant's Guide to OpenBazaar

#### Step One: Set up Store
![Settings Tab](http://i.imgur.com/28L8coh.gif)

1. Click the Settings tab.
2. Enter a new Nickname for your store. This is how other users will see your store.
3. If you want a unique image for your store, put in a URL for an image in the Avatar URL field.
4. Input a Bitcoin address that you control into the Bitcoin Receiving Address field. This is where you receive funds from multisig if the notary needs to take action manually.
5. Describe your store in the Store Description field.
6. Click Save.

#### Step Two: Set Communication Information

![Comms](https://blog.openbazaar.org/wp-content/uploads/2015/04/communicationOB.png)

1. Click on the Communication section.
2. Enter an email address if you want to communicate with other parties via email.
3. If you have a website that you want displayed, enter the URL in the Your Web Site field.
Enter a Bitmessage address if you want to communicate with other parties via Bitmessage.
4. Click Save.

#### Step Three: Create Backup

![backupOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/backupOB.png)

1. Click on the Backup section.
2. Click Create New Backup.

### Step Four: List your Goods or Services

![Create Contract](http://i.imgur.com/jAtFHXV.gif)

1. Click on the Contract tab.
2. Click Add Contract.
3. Input product details, including a title and description of your product, as well as the price (in Bitcoin), the cost of shipping, how many items are available, and what condition the items are in.
4. Add up to three externally hosted images in the photos section.
5. Make sure you click on the Keyword section next to Photos and input keywords that describe your product. This is how users find your items through the search function. If you don’t add keywords, your items cannot be found via search.
6. Click Save. This publishes your product to the network.

#### Step Five: Manage Trade

![sellerOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/sellerOB.png)

1. Manage your sales by viewing the My Sales section under the Orders tab. If you have an order, click on it to display details.
2. If someone purchases your product, the item will display “Buyer Paid.” Please double check the linked multisig account in the order description to verify; at this point a buyer can mark an item as paid without actually paying.
	+ ![productOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/productOB.png)
3. Determine if you trust the notary involved. Since at this point the buyer chooses the notary, if the two parties are colluding, they can cheat you out of the Bitcoin. You can view the notary involved by clicking “Contract Details” in the item description. Early in the beta, we recommend test transactions or small transactions until trusted notaries become established.
4. If you verify the buyer has sent the funds to multisig, and that you trust the notary, then ship the item to the buyer at the address they provided. This address is displayed in the “Shipping Information” tab when viewing the order.
5. Once you’ve shipped the item, input your Bitcoin address into the Shipping & Payment section of the order view, where it asks “Where would you like payment sent to?”
6. Once the buyer receives the item, they should release payment. If they don’t in a reasonable time, contact the buyer and request they release funds. If they are non-responsive, contact the notary involved in the transaction and request they release funds.

### 2.2.2 Buyer's Guide to OpenBazaar

#### Step One: Personalize your Client

![Settings Tab](http://i.imgur.com/28L8coh.gif)

1. Click the Settings tab.
2. Enter a new Nickname for yourself. This is how other users will see you.
3. If you want a unique image for your avatar, put in a URL for an image in the Avatar URL field.
4. Input a Bitcoin address that you control into the Bitcoin Receiving Address field. This is where you receive funds from multisig if the notary needs to take action manually.
5. Click Save.

#### Step Two: Set Communication Information

![communicationOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/communicationOB.png)

1. Click on the Communication section.
Enter an email address if you want to communicate with other parties via email.
2. If you have a website that you want displayed, enter the URL in the Your Web Site field.
3. Enter a Bitmessage address if you want to communicate with other parties via Bitmessage.
4. Click Save.

#### Step Three: Create Backup

![backupOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/backupOB.png)

1. Click on the Backup section.
2. Click Create New Backup.

#### Step Four: Find & Trust a Notary

![Trusting a Notary](http://i.imgur.com/xp8kgug.gif)

Notaries are a vital part of OpenBazaar. They are the third key holder in the 2 of 3 multisig, meaning that if there is a dispute between buyer and merchant, only the notary has the power to work with one of the parties to release the funds. As such, it’s important that buyer and merchant trust the notary not to collude with the other party. In beta we recommend smaller transactions until reputable notaries emerge in the market.

When viewing stores on the Home tab, look for users that offer services. This is only visible if the user offers notary services. If they do, it will display their percentage fee, a description of their services, and allow users to select them as notaries by clicking “Make Trusted Notary.” You can also manually add a notary in Settings if you know their GUID (string of letters and numbers under the store name).

#### Step Five: Find & Purchase Goods or Services

![PipeExampleOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/PipeExampleOB.png)

If you click on an item in a store in the Home tab, a new window opens to give more product details, including the product title, price in Bitcoin, product description, cost of shipping and handling, quantity available, the item’s condition, and up to three photos. There is also a “Raw Contract” button which allows users to view the contract details directly.

![orderdetailsOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/orderdetailsOB.png)

Clicking on “Order Details” on the bottom left will bring you to a screen that allows you to purchase the product. You can determine the quantity desired, and attach a comment for the merchant to see along with your order. If you haven’t entered your shipping address in Settings already, a red warning will ask you to do so before proceeding. The price for the product and shipping and handling are displayed again.

At the bottom the user needs to input a Bitcoin Address that they control. This will be used in case of a refund.

Once this section is completed, the user selects “Choose a Notary.” A list of online notaries that the user has trusted is displayed. If the user hasn’t trusted any notaries, or if none of those notaries are online, they must choose another notary in order to continue.

The user then completes the order by selecting “Submit Order.” This sends the order to the notary and merchant.

#### Step Six: Finish the Purchase

When a buyer views “My Purchases” it will display the status of their orders. If they’ve just submitted an order, the status will indicate “Need to Pay” and the buyer needs to open the order to complete payment.

![needtopayOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/needtopayOB.png)

A QR code is displayed which, if scanned, will input the multisignature address and amount. If the user selects “Pay in your Wallet,” it will open a wallet on their device and pull in the same information. Once the payment is completed, the buyer must manually select “Mark as Paid.” This lets the merchant know to ship the item.

If the buyer marked the order as paid, but the merchant didn’t receive this message due to being offline, the buyer can re-open the order and click on “Resend Payment Notice” when the merchant is online.

Once the item has arrived or service is provided, the buyer can then release the funds from multisig by opening the order and selecting “Release Payment to Merchant.” Again, if the merchant didn’t receive this message due to being offline, the buyer can try releasing again when they are online.

### 2.2.3 Notary's Guide to OpenBazaar

#### Step One: Personalize your Client

1[Settings Tab](http://i.imgur.com/28L8coh.gif)

1. Click the Settings tab.
2. Enter a new Nickname for yourself. This is how other users will see you.
3. If you want a unique image for your avatar, put in a URL for an image in the Avatar URL field.
4. Input a Bitcoin address that you control into the Bitcoin Receiving Address field. This is where you receive funds from multisig if your services are needed.
5. Click Save.

#### Step Two: Set Communication Information

![communicationOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/communicationOB.png)

1. Click on the Communication section.
2. Enter an email address if you want to communicate with other parties via email.
3. If you have a website that you want displayed, enter the URL in the Your Web Site field.
4. Enter a Bitmessage address if you want to communicate with other parties via Bitmessage.
5. Click Save.

#### Step Three: Create Backup

![backupOB](https://blog.openbazaar.org/wp-content/uploads/2015/04/backupOB.png)

1. Click on the Backup section.
2. Click Create New Backup.

#### Step Four: Set up your Notary Details

In Settings, select the Notary section.

1. **Make me a notary**. By clicking Yes in this section, you allow others to choose you as a notary. They will see this option when they click on your store front and see the “Services” tab, or by manually entering your GUID into the Trusted Notaries section. The default option is set to No; users aren’t notaries unless they choose to be.
2. **Fees**. As a notary, you can charge a percentage fee for providing dispute resolution. If the buyer and seller finish their transaction without needing the notary, there is no payment. If the notary is needed to refund buyer or release payment to merchant, then they will receive the percentage from the multisig that they set in this section. A notaries’ fee is visible in the “Services” tab in their store front.
3. **Description of your services**. Notaries can explain their terms and conditions in this area, as well as their credentials and any other information they wish to share.

#### Step Five: Manage your Orders

Note that at this point, offering notary services means you automatically accept all transactions which choose you as a notary. In the future, notaries will be able to screen transactions, or only accept them manually.

If a buyer or seller contacts a notary asking for funds to be released, it’s the notary’s responsibility to do their best to determine which party should receive funds. Once they’ve made their decision and contacted the parties, they can release funds by opening up the order in the notarizations tab.

![OBnotary](https://blog.openbazaar.org/wp-content/uploads/2015/04/OBnotary.png)

In the 0.5 client, the notary has two options. “Refund the Buyer” releases all the funds from multisig to the buyer, minus the percentage fee which is paid to the notary for dispute resolution. “Release money to the Merchant” does the same for the merchant. The notary must click “Send Resolution” for the transaction to process.

