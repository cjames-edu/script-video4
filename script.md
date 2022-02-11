------------------------------------------
# Video 4: Configuring Your Own Node
------------------------------------------

------------------------------------------
## Prerequisites:
  - Previous videos 1-3
  - Mullvad credit (VPN only)
  - Tor (Linux only)
  - Wallet software such as:
  - Feather wallet
  - Monerujo wallet
  
------------------------------------------
 
## Introduction

Greetings everyone!

This is the fourth and final video in our Monero Guide's series on "Getting Started with Monero"

In this video, you will explore account management practices and create different networking methods for enhanced privacy

Let's get right into it ok?


## COIN CONTROL

With Monero, you control coins with three primary security features: ring signatures, input mixing and churning


### RING SIGNATURES

From Moneropedia: 

"In cryptography, a ring signature is a type of digital signature that can be performed by any member of a group of users that each have keys. Therefore, a message signed with a ring signature is endorsed by someone in a particular group of people...

One of the key security properties of Monero is anonymity, in that it should not be computationally possible to determine which of the group members' keys was used to produce the signature"

Ring signatures aren't perfect as they only provide plausible deniability. Attacks can exist where two or more colluding parties attempt to learn information about someone they transact with. They can do so by tracing transaction graphs and performing statistical tests. 

This type of attack is especially effective against those who engage in repeated, uniform transactions with personally identifiable information

For more information about the attacks available to bad actors, please watch the breaking Monero video series

### INPUT MIXING and CHURNING
To prevent risk of exposing your transactions to bad actors, there are two simple things you can do:
Reduce input mixing
Churn outputs to yourself

Input mixing is a shortcut where owned outputs from multiple origins are used to satisfy a new transaction. Mixing known inputs increases the amount of information available to those who are interested in performing statistical analysis to deanonymize transactions.

Churning is simply sending multiple decoy outputs to yourself. In turn, each of those decoy outputs will then be included in multiple ring signatures thereby decreasing risk of bad actors identifying specific transactions

Therefore, you should always churn your inputs before mixing

To manage input mixing and churning options using Feather, go to the COINS tab:
  - To churn, right click on one of your owned outputs and select "Sweep Output"
  - When the new window opens, select "Send to self (churn)"


## NETWORKING

Time to spend some Monero!


### PRIVACY

Maintaining privacy connecting to outside nodes is fine for occasional, one-off payments. Especially if you're using either Feather or Monerujo, which both support The Onion Routing (Tor) Project. 

If your country/state has banned the use of Monero or you don't trust the Internet Service Provider (ISP), then you will want to anonymize interaction with the Monero blockchain. Connecting to your own node through Tor or a VPN will help you manage this potential exposure to bad actors.

You may recall in video two of this series, we explained the types of data made available to both ISPs and node operators. You should keep these things in mind when making decisions about your networking preferences.

Although we are primarily focused on privacy, there are other technical reasons for using a VPN or Tor . For example, your ISP may limit the use of port forwarding or you may not be able to access your router in an administrative capacity.


### CLEARNET and DARKNET

Before we continue, let's review the difference between the Clearnet and Darknet protocols:
  - Clearnet - Publicly accessible Internet
  - Darknet - Internet that can only be accessed with specific software, configurations, or authorisation (onion)

In our previous videos accessing remote nodes, you had been using Darknet over Tor, a free and open-source software which increases anonymity and censorship resistance on the internet

If you are interested in learning more please visit the Tor project website


### PORT FORWARDING

Next you will configure your node to accept Remote Procedure Calls (RPC's) from outside our LAN, first using Clearnet and then Darknet (for Linux users only)

WARNING: RPC  is commonly used by system administrators to remotely control a system for maintenance or to use shared resources. It should almost never be directly exposed to the Internet, as it is frequently targeted and exploited by threat actors as an initial access or back-door vector.

The risk of opening ports is normally tightly bound to the application that is being run through that port. While there are no known vulnerabilities in this Clearnet RPC implementation, it doesn't mean that there isn't any risk.

If this concerns you, we recomend hosting your node over Tor, which does not require opening ports.


### NODE CONFIGURATION over CLEARNET

  - Configure router firewall rules
  - Obtain your external IP

If you are comfortable using transparent Clearnet interactions with the Monero blockchain, just add firewall rules to both the router and the machine hosting your node. 

NOTE: The default port for RPC's is 18081

Add the rules now using our guide from "Using Monero Video Two: Setting up your own node" amd obtain your external IP address by visiting the VPN provider site. We will be using Mullvad as our VPN, although the general ideas apply to all providers.

Using a VPN is equivalent to changing your ISP and depending on the service, you may gain some privacy enhancing features. In the last video we bought a subscription to Mullvad, a privacy focused VPN provider based in Sweden. We specifically chose Mullvad because we are able to configure multiple ports for use with our daemons and we were able to maintain some privacy when buying the subscription.

Unlike services from Nord et al. which require you to provide email addresses and other personally identifying details, Mullvad uses account numbers. To generate an account number we should simply click on the button provided.

  - You will get started on the Mullvad site by creating an account number
  - Now that you have the account, add funds. After purchasing some credit, select the voucher option and enter the details
  - Finally, download the software for your OS and assign ports within your account
  - Remember to verify the download using Mullvad's public key and use their handy installation guide found here:


#### ASSIGN PORTS using MULLVAD
As a VPN user the ports you have access to will be limited. The default ports for P2P connections (18080) and RPC's (18081) will therefore likely be unavailable and you will need to assign ports within your account.

  - After login, use the drop down menu to choose a location and then a city
  - Now click on the settings icon and select "Advanced"
  - There are two types of tunnelling protocols available through Mullvad, OpenVPN and WireGuard

In this video we will demonstrate WireGuard and for more information, use the provided links below:

From your account on Mullvad’s site:
  - Select "Manage ports and WireGuard keys" 
  - Scroll down to "Port forwarding"
  - Select the city you chose within the app and the WireGuard key
  - or "No Key" for those using OpenVPN
  - Click on the green button to add at least two ports (Mullvad forwards up to five)
  - Confirm both under "ACTIVE PORTS"

Next you will open bitmonero.conf to change the default ports to match:

 Confirm the options are labelled:
  - p2p-bind-port= 
  - rpc-bind-port=

Replace the numbers which follow with the P2P and RPC port values from Mullvad amd record which ports you added to each option. Restart your daemon afterwards for these settings to take effect.

Next you will add the port rules to your router firewall and each machine. For guidance, please refer to Video Two: Setting up your own node.

Your Monero wallet software will need the RPC port number and external IP address to connect your node outside the LAN

To find your new IP address, use Mullvad's "check" tool found by selecting "Check for leaks" at the top of Mullvad's web page

Select the drop down menu for Using Mullvad VPN to reveal your external IP address

WARNING: Depending on your ISP and configuration, these IP address may not be static. If you are ever unable to access your node, check your external IP address. 

Finally, add your node to Monerujo or Feather wallet software using their respective methods

In the next section, we'll configure your node for darknet


### NODE CONFIGURATION over DARKNET (LINUX only)

Welcome to the dark side!

Linux users will setup their Monero node as a hidden onion service

Hidden services are a convenient way of sharing local data over the Internet by routing all traffic through Tor and securing it with a private, complex addresses which itself can be further secured using authorization protocols

Although Tor is significantly slower than the Clearnet alternative, it is much more private. Onion addresses are not publicly available and are extremely difficult to guess.

You need to have Tor installed on the machine that hosts your node. Installation depends on your particular distribution, consult Tor for instructions.

After installing Tor on your Linux machine, you will have a dedicated service which starts with your machine each time. 

To check if it's running use the command:
sudo systemctl status tor@default.service

To set up a hidden service, edit the Tor config file typically located in: 

/etc/tor/torrc

If everything is well, your status should show that it's active and running

Using your favourite text editor, add a few lines to define your new service:

HiddenServiceDir /var/lib/tor/monero-service/
HiddenServicePort 18081 127.0.0.1:18081

Now save the changes and close the Tor config file

NOTE: The option HiddenServicePort is a redirect of the already existing service on your host PC. If you have changed the default port for RPC's then change this line to match.

Restart Tor for the changes to take effect and confirm with:

sudo systemctl restart tor@default

Ensure Tor has started correctly and the settings have taken effect once again using the command:

sudo systemctl status tor@default.service

All that's left is to find out the hidden service address which has been issued by Tor. To print this information to the terminal, use the concatenate command with the correct file location:

sudo cat /var/lib/tor/monero-service/hostname

Now you have all the information needed to connect your node using Tor-enabled wallets!


### ADDING NODES TO YOUR WALLET SOFTWARE

Choosing wallet software depends on your OS and privacy concerns. We'll use Monerujo as it works with Android using native Tor integration.

NOTE: Tor functionality also requires Orbot to be installed

To add your node to Monerujo, select the current node first:
  - Once the list has loaded, you will see a + symbol in the bottom corner
  - Enter "Hostname" as your external IP or onion address
  - Enter the RPC port value
  - Give your node a catchy name in your wallet software and leave the "Username" and "Password" fields blank for now
  - Select "Test" to verify the configuration

After entering an onion address, the process may not work immediately. Press OK to save your settings and return to the main menu.

Click the symbol next to your node's name to initialize the Tor integration and you will see the onion symbol indicating you are using Tor to route your traffic

You can use Tor to connect to Clearnet or Darknet nodes although the tradeoff for privacy is a slower network

The process for adding your node to Feather is similar:
  - Select "File" and then "Settings"
  - Select the "Node" tab and then highlight "From custom list"
  - Enter your IP or onion address followed by the port number
  - Click "OK" when you're done and then double click on the node to connect to it

Success! You are now able to use your node from outside of your LAN.


### PASSWORD PROTECTION

Secure your RPC server with usernames and passwords to prevent unauthorized intrusion
 
Open bitmonero.conf
Add each username and password like so: 
rpc-login=username:password

Restart your node for settings to take effect

Good job on setting up your Monero node!


## WHAT'S NEXT?

Visit Monero Guides for scripts and links on each of the videos seen here

For additional reading, both Zero to Monero and Mastering Monero are valuable resources that offer a great level of detail

Thank you for  supporting Monero and using Monero Guides

We hope you feel confident using these tools to continue your journey with Monero

Goodbye for now
