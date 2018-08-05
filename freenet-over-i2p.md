Freenet Over I2P

How to set up I2P and Freenet to create an anonymous darknet

Uploaded: 2018-08-01 (UTC)
Introduction
How it works
Instructions
Preparation
Installation
Configure I2P and Garlicat
Configure I2P
Create an I2P hidden service (server) tunnel
Create an I2P client tunnel
Find your Garlicat I2P ID
Run Garlicat as a service
Configure Freenet
Start your new Freenet node and run the configuration wizard
Configure Freenet to run over I2P
Check your node reference for anonymity:
Final steps
Save your node reference for sharing
Configure Freenet for hybrid node operation
Share node references and add friends
Optional Freenet patch
Testing, Performance and Usage
Alpha testing
Beta testing
Startup and shutdown
I2P eepsites and hidden services
FAQ
Support
Links
Introduction

Freenet is typically run in “opennet” mode where it connects first to some seed nodes, then to several random strangers (your peers.) This is the most common way to run Freenet as most people don’t already know others they can connect to, so they just let Freenet choose their peers from the available pool of other users. This presents some security risks: if one of those random peers happens to be an adversarial node they now have your IP address, and if more than one are adversarial they could perform what’s known as a “surround attack” and possibly find out what you’re doing on Freenet, breaking both your anonymity and privacy. In any event, total strangers can easily see that you’re using Freenet and you’d probably feel more comfortable if they couldn’t.

The recommended solution is to run Freenet in “darknet” mode instead, where rather than connecting to random strangers you form a local, friends-and-family, “small world” network composed of people you actually know and trust, with whom you exchange node references, and then only connect to each other rather than to the seed nodes and to random peers. While doing so greatly increases your security, the difficulty is in forming this network in the first place.

It is recommended that for best performance and anonymity such a network have at least 10 members. How many of us personally know 10 people who 1) we can trust enough to let them know we use Freenet, 2) have both the computer know-how and the interest to want to do likewise, 3) can be relied on to follow best practices regarding security and anonymity, and 4) are likely to leave their computer and Freenet running 24x7? Not very many of us unless we happen to have a large circle of family and friends who are mostly computer geeks and privacy advocates. We would be lucky if we knew one or two such real-life friends.

Anonymous networks such as Freenet are generally private, non-social networks: we use Freenet, I2P or TOR specifically because we don’t want others to know what we’re doing online or even that we’re using an anonymous “deepnet” at all: not because we’re necessarily doing anything illegal, but because of our privacy and anonymity concerns, and because we don’t feel we can trust others such as the authorities, ISPs, big businesses, or sometimes even our own family and personal friends not to do anything illegal or unethical. This is especially an issue in some countries with repressive political regimes, which are where anonymous, private networks such as Freenet are needed the most.

This freesite offers a possible solution: running a Freenet darknet over I2P so that your node reference (noderef) information contains no identifying information such as your actual routable Internet IP address. These noderefs can then be shared anonymously with total strangers over Freenet if you wish without risk to your own anonymity (see the links below for a freesite created just for that purpose) or shared between acquaintances whom you may know slightly but not well enough to trust as normal darknet friends (such as classmates, co-workers, members of a club you belong to, those whom you’ve recently met, members of your Linux users group, people you meet at LAN parties, drinking buddies, or any other casual but not close friends.)

If they also configure Freenet to run over I2P you all can safely form your own Darknet without the worry of whether or not these casual friends and acquaintances can be trusted with your IP address or other personal information: no one should be able to easily tell your noderef from any of the others on the darknet, and even if they do they’ll have absolutely no identifying information about you nor will anything be divulged should one or more of your darknet’s members have their computer seized, lost, stolen, hacked or otherwise compromised, or if your darknet is infiltrated by an adversary. Once you are connected to a sufficient number of darknet “friends” you will no longer connect to any regular opennet random peers using your actual IP address when running a hybrid node, as is recommended here, hiding the fact that you’re even on Freenet at all.

FYI, I2P was preferred over TOR as the anonymous network of choice to due to the fact that an increase in overall traffic and number of users on I2P increases its network routing efficiency as well as everyone’s anonymity, so they welcome users who use I2P for higher-traffic activities such as bit torrenting for example, while TOR discourages these activities on their network as such things would consume too many of its resources and could cause problems for other users. It comes down to how I2P’s “garlic routing” works vs. TOR’s "onion routing."

How it works

OnionCat creates a transparent IPv6 layer on top of Tor’s hidden services or I2P’s tunnels. It transmits any kind of IP-based data transparently through the Tor/I2P network on a location hidden basis. You can think of it as a peer-to-peer VPN between hidden services.

OnionCat is a stand-alone application which runs in userland and is a connector between Tor/I2P and the local OS. Any protocol which is based on IP can be transmitted. Of course, UDP and TCP (and probably ICMP) are the most important ones but all other protocols can also be forwarded through it.

OnionCat opens a TUN device and assigns an IPv6 address to it. All packets forwarded to the TUN device by the kernel are forwarded by OnionCat to other OnionCats listening on Tor’s hidden service ports or I2P’s server tunnels. The IPv6 address depends on the onion_id or the i2p_id, respectively. The onion_id is the hostname of the locally configured hidden service (see tor(8)). Depending on the configuration of Tor the onion_id usually can be found at /var/lib/tor/hidden_service/hostname or similar location. The i2p_id is the 80 bit long Base32 encoded hostname of the I2P server tunnel.

-from gcat’s man page.

Essentially, in Darknet mode Freenet overrides your own IP address with the IPv6 address from Garlicat so all darknet Freenet traffic is routed through this address. Garlicat forwards that data to your I2P socks proxy for outbound traffic and from your I2P hidden service (server) for inbound traffic. When everything is properly configured your Darknet node reference data contains your Garlicat IPv6 address rather than your Clearnet IPv4 address, effectively anonymizing your Darknet Freenet node and making it safe to share your node reference with those other than close and trusted personal friends. This allows people who don’t have enough friends who use Freenet to still form Darknets.


Instructions

Note: these instructions assume that you’re using a Debian- or Ubuntu-based Linux distribution. Users of other operating systems will have to figure out their particular installation and configuration details. The information presented here should still serve as a general guide to the procedures and steps to follow.

Caution: These instructions, and the process of running a Freenet darknet node over I2P to hide your IP address, are experimental. Use them at your own risk. It is up to you to be careful and ensure that you’re not revealing any information that could be used to identify you.

Preparation

1. Make sure that UPnP is enabled in your Internet router before you start. If it isn’t, enable it and save the configuration. You may also need to reboot your router for it to take effect.

2. In your web browser, click on “File/Save Page As...” (in Mozilla browsers) and save a local copy of this freesite to your hard drive then shut down Freenet to avoid any possible interference with your new node or with I2P or Garlicat.

3. Read these instructions thoroughly and familiarize yourself with the steps prior to beginning. You can do a dry run without actually changing anything just so you know where everything is, such as on the I2P router console’s pages. If you follow these instructions exactly, everything should work as expected.



Installation

1. Install the latest version of i2p for your operating system:
https://geti2p.net/en/download
(Linux users should install i2p from their distribution’s repositories if it’s available. If not, Debian and Ubuntu (and their derivatives such as Mint) users should follow the instructions in the i2p download page to add the i2p PPA then install the package with apt or Synaptic. Users of other Linux distros or other operating systems should download and install the version that’s best for them. You should do a headless installation if possible.)

2. Install the Onioncat version for your OS:
https://www.onioncat.org/download/
(Linux users should install onioncat from their distribution’s repositories if it’s available. If not, your best bet is to clone the latest source code from GitHub, compile it and install it yourself. Doing this should also work on OS/X, and on Windows if Cygwin is installed.) Onioncat now includes Garlicat which is needed to tunnel UDP packets over i2p.

3. Install OpenSSL for your OS if it isn’t already installed:
https://www.openssl.org/ or http://gnuwin32.sourceforge.net/packages/openssl.htm
(Linux users should already have OpenSSL installed: if not they should install it from their distro’s repositories if it’s available. OS/X also probably has OpenSSL already installed.) OpenSSL will be needed later to create a random darknet node name.

4. Install a new instance of Freenet:
https://freenetproject.org/pages/download.html
Install it into a different directory than that of your existing node. Go through the initial Java installation wizard, but not through the configuration wizard in FProxy (in your web browser) yet. We’ll be configuring it specially for darknet operation later. You can shut down this new Freenet node from the command line: ./run.sh stop

(We’re installing a new instance of Freenet for three reasons: 1) if something doesn’t work properly you’ll still have your original, working Freenet node available so you can ask for help, and 2) even if everything goes smoothly, Freenet in darknet mode won’t even work yet anyway until you have at very least one darknet “friend” to connect to. We don’t want to put your existing node offline in the meantime. 3) If you take a node that’s been running in opennet mode then change it to high (darknet) security its node reference may leak your Internet IPv4 or IPv6 address even after setting it up to run over I2P. A brand-new installation, configured as high-security/darknet over I2P right from the start, won’t do that.)



Configure I2P and Garlicat

Configure I2P

1. Start i2p as a service. On Linux you must run sudo dpkg-reconfigure -plow i2p and configure i2p to run as a daemon, use the default user name it will run as, allocate at least 256MiB RAM, enable sandboxing of i2p with AppArmor, then type sudo service i2p start. On Windows, create a shortcut to i2p.exe in your startup folder then reboot.

2. Open your i2p console at http://127.0.0.1:7657/console so you can configure i2p and add Garlicat tunnels. (Recommended that you bookmark this page to easily come back to it later, which you will need to do.)



If you see a lot of icons in the main I2P right-hand pane rather than a screen like the above image, click on the I2P logo at the top of the left sidebar to open the detailed router console screen as shown above rather than the default I2P screen (you want to be on http://127.0.0.1:7657/console rather than http://127.0.0.1:7657/home).

Scroll down until you can see the TUNNELS section near the bottom of the left sidebar. It may say “Rejecting tunnels: Starting up”. This is normal when first starting i2p. Wait at least 10 to 20 minutes for your i2p node to bootstrap and become integrated with its network.

If i2p is still rejecting tunnels after about 20 minutes look at your network status, which is between BANDWIDTH IN/OUT and I2P SERVICES in the sidebar. If it says “Network: hidden” go to http://127.0.0.1:7657/confignet and check your network settings. If the "Hidden mode – do not publish IP (prevents participating traffic)" radio button is selected click on “Use all auto-detect methods” then scroll to the bottom of the page and click “Save changes”.



If your I2P network status is “Network: firewalled” that means that I2P is unable to receive inbound packets. It could be that your UPnP isn’t working in which case you’ll need to forward your I2P router’s port (see below), both UDP and TCP, in your Internet router. Also make sure that you don’t have interference by software firewall in your operating system. It could possibly be that your ISP has blocked that particular port upstream too, in which case try using a different port.

3. Wait a few minutes. Once your I2P network status is “Network: OK” and your tunnels status is “Accepting tunnels” I2P should be working and you may continue.

Note: you may wish to forward i2p’s UDP and TCP shared port in your Internet router in case there are UPnP issues or if you don’t want to allow UPnP to open ports whenever requested for security reasons. Your i2p port can be found near the bottom of http://127.0.0.1:7657/confignet just above the network settings. You may need to restart your Internet router after doing so for the port-forwarding to take effect.

4. Go to http://127.0.0.1:7657/config, click on the Bandwidth tab and enter your actual upstream and downstream bandwidth as tested and measured on a service such as speedtest.net with no other current Internet activity. Recommended: allocate at least half of your actual bandwidth (50% in the bottom setting) to I2P and Freenet. Click “Save changes”.



Create an I2P hidden service (server) tunnel

1. Go to http://127.0.0.1:7657/i2ptunnelmgr and look for I2P HIDDEN SERVICES. Look for "New Hidden Services in the lower right corner:



2. Change the “New hidden service” type from HTTP to Standard and click “Create”.

3. A new page will open. Enter a name for your new hidden service, i.e. “Freenet Inbound”. Under “Auto Start Tunnel” check the box next to “Automatically start tunnel when router starts”. In the “Port” field, which is populated by the word “Required” in red text, enter 8061. Leave everything else at the default settings.



4. Scroll to the bottom of the page and click “Save”.

Create an I2P client tunnel

1. On the same page, look for I2P CLIENT TUNNELS. In the lower-right corner you’ll find “New client tunnel” with a default value of “Standard”. Change this to “Socks 4/4a/5” and click “Create”.

A new page entitled “NEW PROXY SETTINGS” will open.

2. On top, enter a name, i.e. “Freenet Outbound”. Enter port number 9051. Check the autostart checkbox. Leave the other settings at their defaults.



3. Scroll to the bottom and click “Save”.

Find your Garlicat I2P ID

1. Scroll up to “I2P HIDDEN SERVICES”. Find your Freenet Inbound hidden service. Look for where it says “Base 32”. Beneath that is the word “Address:” followed by a string of what appears to be random text and a “b32.i2p” extension.



2. Copy this string of text and paste it into a text editor. Save the file as freenet-i2p.txt.

3. Delete all but the first 16 characters and append the extension “.oc.b32.i2p”. This will be your Garlicat I2P ID.
Example:
Base32 Address: ktmhezukbp6jyuldedcjmso6p4e6aegeikqli2mne67hnttgmvfa.b32.i2p
GarliCat I2P-ID: ktmhezukbp6jyuld.oc.b32.i2p

(I find it helpful to copy the entire line, paste it into a text editor, delete the “Address:” at the front then insert a space after every four characters until I have four groups or four. Then I just delete the rest of the line, remove the spaces and type “.oc.b32.i2p” at the end. This prevents accidentally miscounting and ending up with 15 or 17 characters instead of 16.)

Save your Garlicat ID in freenet-i2p.txt.



Run Garlicat as a service

(Garliccat is part of Onioncat, which you’ve already installed.)

1. Edit /etc/default/onioncat as sudo or root and change the appropriate lines so they match the following:

ENABLED=yes
DAEMON_OPTS="-d 0 -I “your Garlicat I2P ID”" (Replacing “your Garlicat I2P ID” with the one you made above.)
Example: DAEMON_OPTS="-d 0 -I ktmhezukbp6jyuld.oc.b32.i2p"

2. Save the file then restart Onioncat:
sudo service onioncat restart

3. Find your Onioncat (Garlicat, actually) IPv6 address:
gcat -i your Garlicat I2P ID
Example: gcat -i ktmhezukbp6jyuld.oc.b32.i2p
Copy this address to freenet-i2p.txt



Configure Freenet

Start your new Freenet node and run the configuration wizard

1. Start your new Freenet node and go through the browser based setup wizard. Choose “Details settings: (custom)” for the security option. On the subsequent pages of the wizard:

Enable or disable the UPnP plugin depending on whether or not you need it.
Choose “Only connect to your friends”
Choose “High” for “Protection against a stranger attacking you over the internet”
Click the “I trust at least one person already using Freenet” checkbox.
For “Protection of your downloads...” pick any option you want. (Recommended: no encryption as you should already have full-disk encryption for your operating system’s partition and any other hard drive partitions you may have.)
Create a random node name that your darknet friends will see:
At a command prompt run openssl rand -base64 20 or in Windows openssl.exe rand -base64 20, copy its output except for the equals sign at the end, and paste that as your node name. This will help to preserve your anonymity as otherwise you may subconsciously provide a clue about yourself if you make up your own name.
Also save your node name in freenet-i2p.txt.
Pick the datastore size that you want. At least 50GiB recommended.
Choose your bandwidth limits. Keep them somewhat low for now: you can fine-tune them in your Configuration/Core Settings later.
The node will now be started but have no connections. There will be warnings about this. Just ignore them.

2. Go to Status/Internet connection and copy your Darknet port number. Paste it in your freenet-i2p.txt file and save it.

3. You will need to forward this port in your Internet router if possible so you may as well do that now.

Configure Freenet to run over I2P

The following settings in Freenet need to be changed:

1. In “Configuration/Core Settings” (make sure you have clicked “Switch to advanced mode”) change “IP address override” to your Garlicat IPv6 address retrieved in the previous section. Apply the changes. You may also want to apply the settings recommended in the Freenet Quickstart freesite listed below in the links section for improved overall performance.

2. Shut down Freenet and edit the wrapper.conf file in the Freenet installation directory. Change the line that contains java.net.preferIPv4Stack=true to java.net.preferIPv4Stack=false. Example:
wrapper.java.additional.3=-Djava.net.preferIPv4Stack=false

3. Edit the freenet.ini file in your darknet Freenet installation directory. Change the following two lines at the end to these values:

node.load.subMaxPingTime=2500
node.load.maxPingTime=5k)

and change these two lines:

node.bindTo=0.0.0.0
node.opennet.bindTo=0.0.0.0

replacing 0.0.0.0 with your Grarlicat IPv6 address.

Also change fproxy.port=8888 to a random port number between 5001 and 49151 to protect against a vulnerability that may allow attackers to see which freesites you’ve visited. See
@USK@kSloth~SrrdJfe8qoumuiY7tWSVOpQav7usAwMB7PqE,JJRmnrN1EoxLcwsmAEpVa~MkygHBs8orW4QEud9CCG4,AQACAAE/fmsarchive/1435/BE/BE07FEF8_356E_4093_873B_A98795876E4D_6kzjmQCFtZFFEJ0WThb29r63T5JkJg2Xy5hZSvItG1A.html@ for information about this vulnerability.

4. Save the freenet.ini file and restart Freenet. There will be a warning about “Unknown external address”. Ignore this as you’ve explicitly set one. There is a Freenet patch provided later that eliminates this warning.

5. When Freenet was installed it created a cron job that automatically starts Freenet at boot. If you would prefer to launch Freenet yourself you can find the file containing the cron job in /var/spool/cron/crontabs. You must have elevated privileges to enter the crontabs directory. Edit that file to remove the line or lines that auto-start Freenet on boot.

Check your node reference for anonymity:

1. In Freenet’s FProxy, click on Friends, scroll down to “My Node Reference (as text)”, right-click on “(as text)” and open it in a new tab.

2. Copy the text after "physical.udp=="

3. At a command prompt type echo “physical.udp base64 here” | base64 -d replacing physical.udp base 64 here with the text you just copied (shift+ctl+v or edit/paste in your command window.) Be sure to enclose the physical UDP string inside the quotation marks.

4. Check it to ensure that your actual IP address isn’t revealed. All I2P link-local IPv6 addresses should begin with “fxxx:” (with the "x"s representing various characters) while actual routable (clearnet) Internet IPv6 addresses begin with “2002:” and local private LAN IPs generally begin with “fc00:”. You probably are familiar enough with IPv4 addresses to recognize your Internet IP. (What you’re looking for here are any routable clearnet IPv4 or IPv6 addresses.)

Note: you will see your own private LAN’s IP address for your computer in the noderef as well as localhost address 127.0.0.1. That’s fine: almost all home Internet routers use a local LAN IP convention of 192.168.x.x (or possibly 10.x.x.x. or perhaps a private IPv6 network) It’s virtually impossible to identify your computer based on this address as private address ranges are very few, so they’re used in common on virtually all private home LANs. If it shows that your computer is on, say, 192.168.1.3, it’s just one of several thousands that happen to be using that same local IP address. You will also find your own darknet UDP port listed. This is needed by Freenet so friends' nodes know what port to use to communicate with your node. I would rather that it didn’t reveal that port number but as long as your IP address is obfuscated, which it is, that one small tidbit of information about your node shouldn’t really matter vis-á-vis your anonymity.

5. It isn’t a bad idea to base64-decode everything in your node reference that appears to be so encoded, just to be sure it isn’t revealing anything it shouldn’t, before you share it with others.

6. Copy your ark number and ark.pub.URI:

ark.number=0
%{color: brown}ark.pubURI=SSK ... AQACAAE/ark

and use this to create a USK key that you can fetch in FProxy to view the information contained in your public ARK:

USK ... AQACAAE/ark/0/@

Again, you’re checking to ensure that your actual IP address is not being revealed.



Final steps

Save your node reference for sharing

1. On Freenet’s Friends page, right-click either (or both) on “My Node Reference” and “(as text)” and save your current node reference, either (or both) as a myref.fref file or as a plain text file.

2. Keep these files in a safe place as they are the ones you will either give to friends or acquaintances along with a locally-saved copy of this freesite for instructions, or, in the case of the text version, you’ll share its contents anonymously on Freenet if you choose to do so, whether by posts in FMS or Frost-Next or by adding it anonymously to the user-editable freesite in the Links section below.

Configure Freenet for hybrid node operation

If some of the nodes on the I2P darknet run as hybrids, accessing both opennet and darknet, everyone else on that darknet will be able to access all of Freenet including freesites, Sone, FMS, Frost, Freemail, WoT, etc. Be advised that RUNNING A HYBRID NODE WILL EXPOSE YOUR ACTUAL IP ADDRESS TO YOUR CONNECTED DARKNET FRIENDS. If you don’t mind this, or if you use additional security measures such as running Freenet over a VPN or on a VPS, volunteering to run a hybrid node will benefit everyone.

1. Click on Configuration/Security Levels and change it from High to Normal.

2. Forward your Opennet port in your Internet router if possible.

That’s it!

Share node references and add friends

This is the moment you’ve been waiting for! Share your I2P-anonymized node reference file that you’ve saved with others, add each other to your new darknet, and when you have at least 3 to 5 other participants start using your new Darknet Freenet node instead of your old Opennet one. Enjoy your increased security!

When adding darknet “friends”, set their “How much do you trust this friend?” security level to High to maximize performance, or leave it at Normal if you prefer. For “Do you want your other friends to be able to see and connect to this friend?” you should usually choose yes. Leave the “Enter description” field blank if adding anonymous and unknown people as friends, otherwise you may wish to enter something to let you identify this person (i.e. if it’s an acquaintance who gave you a copy of their noderef.)



Optional Freenet patch

When running an I2P-based node Freenet sees that the Garlicat IP address is a link-local address. Some places in the Freenet code base check for this and reject it as not being a valid routable Internet address. In the FProxy user interface a large warning appears at the top of each page that it couldn’t find the external IP address of the node. The other issue is that local addresses aren’t counted for bandwidth statistic reporting. The bandwidth box on the statistics page is empty as a result.

This Freenet patch (written by thebluishcoder, aka doublec, for Freenet over Tor) provides a workaround for these two issues. The patch is optional as the node works without it but it’s a useful improvement if you plan to run an I2P- based node long-term as it eliminates that annoying warning at the top of FProxy as well as fixing the bandwidth statistics. As with any Freenet patch, you should check this one before applying it to ensure that it’s not doing anything nefarious. Download the above file then rename it to onioncat.patch (remove the “.txt” extension: it was required to allow you to view the file in FProxy prior to downloading it, should you wish to do so.)

You may apply any other patches to fred (the Freenet REference Daemon) that you wish at the same time. The easiest way to patch Freenet is by using FNAutopatch (see the links section below.) FNAutopatch is a shell script so you may easily review it prior to runnng it. Note that it assumes that you’ve installed Freenet in its default directory at $HOME/Freenet. If you’ve installed Freenet elsewhere you’ll need to create a link (named Freenet) to your actual installation directory inside your home directory for the autopatch script to work. Alternately, install another instance of Freenet letting it install to its default directory, then copy its patched .jar file to your actual Freenet directory. If you do this you’ll have to delete the existing link named freenet.jar that links to freenet-stable-latest.jar and create a new link, named freenet.jar, to the patched jar file. (Do this with Freenet shut down, of course.)



Testing, Performance and Usage

Alpha testing

I have set up I2P, Onioncat/Garlicat and Freenet on two computers and was able to connect them as darknet friends. When only one Freenet node was changed to hybrid mode I was able to browse freesites on the other machine, and they loaded reasonably quickly. However, this is with two machines on the same LAN so your speed and performance results in the real world may differ. On the darknet-only (high security) I did get a large red warning message at the top of FProxy saying that I only have one peer connection and need at least 3 to 5, until I also changed it to hybrid and it began connecting to opennet peers besides the one darknet peer.

I have also applied the patch that eliminates the warning message at the top of every FProxy page saying Freenet can’t find your IP address and prompting you to enter an IP hint. I got tired of looking at that warning all the time. I examined the patch first: it seems safe and it works.

Beta testing

This will require several volunteers to establish a multi-node, larger darknet between people not on the same LAN and test its operation and performance in real-world conditions. Please feel free to do so. You can report your results in either the FMS or Frost board mentioned below.

Startup and shutdown

1. When shutting down your computer, in addition to shutting down Freenet you must open the I2P console and gracefully shut down i2p as well. This could take up to 11 minutes, perhaps longer, depending on how many active tunnels are currently running through your i2p node.



2. When starting your computer, open the I2P console and wait for both the “Network: OK” and “Accepting tunnels” statuses prior to starting Freenet. If Freenet won’t connect at this point or is having performance issues it’s probably due to I2P not having become integrated into its network yet. You may have to wait several minutes, or up to an hour, to achieve peak performance. That’s why it’s best to keep both i2p and Freenet running all the time if you can. The longer you run i2p between system shutdowns the faster it is to restart and become fully operational as, like Freenet, it remembers nodes that it has connected to in the recent past rather than having to bootstrap from scratch.

I2P eepsites and hidden services

If you want to explore the I2P network’s world, be aware that to access its “eepsites” and other hidden services you must use a different web browser (or browser profile) than that in which you access the I2P console. It must be configured to use a default proxy of 127.0.0.1:4444.

FAQ

Q: In hybrid mode I still connect to random opennet strangers as peers in addition to my darknet friends. Isn’t this still the same security risk as connecting in pure opennet mode without I2P and a darknet?
A: No. Even if one or more of your opennet peers manages to identify you they would still have no way to tell whether any traffic coming from your node actually originated from you and not from one of your darknet friends. This vastly increases your plausible deniability as well as your security as surround attacks would no longer work. They can only prove that any given traffic originated from somewhere in your darknet, not prove that it came specifically from your node. That being said, running as a hybrid will expose your IPv4 Internet address to those darknet friends who have connected to your node.
Q: I don’t want to connect to any opennet strangers at all! Can I just leave my node at high security instead of hybrid?
A: You certainly can as long as somebody else runs a hybrid node to allow you access to all of Freenet. If nobody on your darknet would ever want to access the broader Freenet network at all, everyone on it can set their Freenet nodes to high security then those on the darknet can only send messages and share files with one another. This could be extremely useful in certain situations as, if node references have been anonymized via I2P and Garlicat, even if one member has his computer seized by the authorities they would have no information by which to identify the other members, given that the members all practice good OpSec and don’t identify themselves via private messages or something, and if the authorities can’t coerce their captive into identifying the others.
Q: What about performance? Won’t Freenet be a lot slower due to the additional latency of running it on top of I2P?
A: This remains to be seen, but Freenet handles routing differently when you’re connected only to friends instead of to strangers which should offset any performance issues caused by running over it I2P. Also, the longer you leave I2P running the more tunnels it will discover and use and the faster it will be. We will find out for sure when we start getting more and more people to share their I2P+Garlicat-anonymized darknet node references and form one or more larger darknets.
Q: If I share my node reference anonymously on that freesite in the links below, and add other people’s noderefs as friends, I’m still connecting to strangers. I heard that creating a darknet with people that you don’t actually know and trust is dangerous.
A: The danger in forming an anonymous darknet with total strangers such as other Freenet users is that normally in Freenet your node reference would contain your own Internet IPv4 address, encoded in base 64 but easily decoded by anyone. Sharing that information online breaches your anonymity as you have no idea who might get hold of it or what they might do with that information. You could end up being on a darknet consisting of mostly adversaries for all you knew, and they’d all know your IP address. If you run your Freenet darknet node as an I2P hidden service and create a random node name as per the instructions here, your noderef file doesn’t reveal your true IP address (unless you configure your node as a hybrid) nor any other clues about your identity so your anonymity is still safe if you should choose to anonymously share your noderef with strangers.
Q: How many darknet peers or “friends” should I add? Is there a “sweet spot” where performance is best? Is there a practical limit?
A: FProxy’s Friends page says you should have a minimum of 3 friends connected at all times and recommends 5 to 10. It certainly doesn’t hurt anything to add more than that. As stated above, if your upload bandwidth setting limits your total possible opennet peer connections to, say, 42 and you have at least that many darknet friends added, even when running a hybrid node you will connect only to your darknet friends yet you (and everyone else on your darknet) will also be able to access freesites, FMS, Sone, etc. and nobody will be able to see that you’re on Freenet. So the minimum is 5. with at least 3 of the 5 being connected at all times, and the maximum is 85 connected friends (which is the maximum limit of peer connections starting with Freenet build 1479.) You can add more than 85 if you wish though you may rarely or never actually connect to some of them. There is no sweet spot beyond which it isn’t necessary to go.
Q: I must have messed something up because I can’t connect to any Darknet friends/I can’t load any Freesites/Freenet won’t start/I keep getting an error message/I don’t connect to Opennet peers in hybrid mode/I have other Freenet problems.
A: Delete your newly-installed Freenet, reinstall it and reconfigure it according to these instructions. It will assign a different Darknet port so you’ll need to 1) forward it in your Internet router and 2) save and share new copies of your node reference files. When you later configure it as a hybrid node you’ll also need to change the forwarded Opennet port in your router to the new one. Freenet seems to store some of its configuration information someplace in addition to freenet.ini and wrapper.conf and I haven’t been able to find out where. The only solution I’ve found to fix a malfunctioning Freenet node is to delete it and start over, though just with the Freenet installation and configuration not with I2P and onioncat. The safest thing to do, once you have Freenet working, is not change any of its settings and just leave it alone.
Support

Please post support questions in the FMS (or jfms) board freenet, or alternately in the Frost-Next board freenet_keyed using these keys:

Public:
SSK@mmqJJCIzU9B5EGNipwH8VpsHp~0oMGV5xq9vWNNbGAk,Ae4FMM0xj2KlwTMthKHXIg5HecsRJh2T6DippCvMIws,AQACAAE
Private:
SSK@fPu234im8x6ix6fjU-lXUjJSSepaCf0uz-05T07R-es,Ae4FMM0xj2KlwTMthKHXIg5HecsRJh2T6DippCvMIws,AQECAAE


Links

Anonymous Node References, a user-editable freesite where you can anonymously share your node reference and find others to form a darknet anonymously over Freenet.

(Mirror) Freenet over TOR. For reference only, a mirror of doublec’s (a.k.a. TheBluishCoder) flog post from 2016 about running a Freenet darknet as a TOR hidden service.

The Kitty’s Corner, home of Frost-Next, FNAutopatch, StaticSiteCompiler and other Freenet utilities and apps.

Freenet (Quick)start, has a large section explaining how to configure Freenet for speed and stability.

Freenet Privacy and Software Site, list recommended best practices for protecting your privacy and anonymity on Freenet.



This Freesite Made By:

poets@iosi5ghfwuy4mfxk235qlumbam5k2z7dwk5mynqjxvb6ycbtyq6a.freemail



Bookmark this freesite  View the source  View the style  Extracted keys  Freesite files  Check for updates

This freesite was created using the Sharesite plugin
