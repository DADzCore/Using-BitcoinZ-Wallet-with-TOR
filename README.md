# Using-BitcoinZ-Wallet-with-TOR
Using BitcoinZ Wallet with TOR
Tutorial: Using the BitcoinZ Wallet via Tor and Connecting to an .onion Node
Introduction
This tutorial explains how to set up the BitcoinZ wallet to connect to the network via Tor and specifically connect to an .onion node to maximize anonymity. By following these steps, you can use BitcoinZ in a more private and secure way. This tutorial is written for Windows users, but the concepts can be adapted for other operating systems.
Prerequisites:
A computer running Windows (steps can be adapted for Linux or macOS).

An internet connection.

The BitcoinZ wallet (version 2.1.0 or later recommended).

Tor (via Tor Browser or a manual installation).

Goal: Configure your BitcoinZ wallet to connect to the network via Tor and specifically connect to the following .onion node: 7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989.
Step 1: Download and Install Tor
Tor is a network that allows you to anonymize your connections. We’ll use it to ensure your BitcoinZ wallet connects to the network privately.
Option 1: Use Tor Browser (Recommended for Beginners)
Download Tor Browser from the official website: torproject.org.

Install Tor Browser by following the instructions (e.g., in C:\Users\<your_user>\Downloads\tor-browser).

Launch Tor Browser and ensure it connects to the Tor network:
Once Tor Browser is open, go to check.torproject.org.

You should see a message confirming that you are connected to the Tor network.

Keep Tor Browser running throughout this tutorial. Tor Browser uses port 9050 for SOCKS by default and port 9151 for the ControlPort.

Option 2: Install Tor Manually (For Advanced Users)
Download the Tor Expert Bundle for Windows from torproject.org (e.g., tor-expert-bundle-windows-x86_64-14.5a5).

Extract the file to a folder, e.g., C:\Users\<your_user>\Downloads\tor-expert-bundle-windows-x86_64-14.5a5.

Create a torrc file in the tor folder:
Open a command prompt:

cd C:\Users\<your_user>\Downloads\tor-expert-bundle-windows-x86_64-14.5a5\tor
notepad torrc

Add the following lines:

SocksPort 9050
ControlPort 9051

Save the file (ensure it’s named torrc and not torrc.txt).

Launch Tor:

tor.exe -f torrc

Wait for Tor to connect. You’ll see messages like:

Bootstrapped 0% (starting)
...
Bootstrapped 100% (done)

This may take 1-2 minutes. Keep the window open.

Step 2: Download and Install the BitcoinZ Wallet
Download the latest version of the BitcoinZ wallet from the official website or GitHub: github.com/btcz/bitcoinz.
For example, download bitcoinz-windows-2.1.0.zip.

Extract the file to a folder, e.g., C:\Users\<your_user>\AppData\Local\BTCZCommunity\BTCZWallet\Data.

Ensure the files bitcoinzd.exe and bitcoinz-cli.exe are present in this folder.

Step 3: Configure BitcoinZ to Use Tor
We’ll configure BitcoinZ to connect to the network via Tor and specifically connect to the .onion node.
Create or Edit the bitcoinz.conf File:
Open a command prompt:

notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\bitcoinz.conf

If the file doesn’t exist, Notepad will create it.

Add the following lines:

proxy=127.0.0.1:9050
torcontrol=127.0.0.1:9051
listen=1
bind=127.0.0.1
listenonion=0
torstartupdelay=120
addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989

proxy=127.0.0.1:9050: Tells BitcoinZ to use Tor as a proxy (Tor’s SOCKS port).

torcontrol=127.0.0.1:9051: Uses Tor’s ControlPort (9151 if using Tor Browser, 9051 if using manual Tor).

listen=1 and bind=127.0.0.1: Allows BitcoinZ to listen on 127.0.0.1:1989.

listenonion=0: Disables BitcoinZ from creating its own onion service (to avoid errors).

torstartupdelay=120: Gives Tor time to start before BitcoinZ connects.

addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989: Adds the .onion node to connect to.

Save the file.

Launch BitcoinZ:
Ensure Tor is running (Tor Browser or manual Tor).

Launch BitcoinZ:

cd C:\Users\<your_user>\AppData\Local\BTCZCommunity\BTCZWallet\Data
bitcoinzd.exe

Wait 5-10 minutes for BitcoinZ to sync with the blockchain.

Verify BitcoinZ is Using Tor:
Run:

bitcoinz-cli.exe getnetworkinfo

Look for the "networks" section. You should see:

"networks": [
  {
    "name": "ipv4",
    "limited": false,
    "reachable": true,
    "proxy": "127.0.0.1:9050",
    "proxy_randomize_credentials": true
  },
  {
    "name": "ipv6",
    "limited": false,
    "reachable": true,
    "proxy": "127.0.0.1:9050",
    "proxy_randomize_credentials": true
  },
  {
    "name": "onion",
    "limited": false,
    "reachable": true,
    "proxy": "127.0.0.1:9050",
    "proxy_randomize_credentials": true
  }
]

This confirms BitcoinZ is using Tor for all connections.

Verify Connection to the .onion Node:
Run:

bitcoinz-cli.exe getpeerinfo

Look for a peer with the address "addr": "7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989". If you see it, you’re connected to the .onion node.

Step 4: (Optional) Switch to an Onion-Only Setup
If you want your node to connect only to .onion peers (and not to ipv4/ipv6 peers), you can enable onlynet=onion.
Edit bitcoinz.conf:
Open bitcoinz.conf:

notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\bitcoinz.conf

Add or uncomment the following line:

onlynet=onion

Save the file.

Restart BitcoinZ:

bitcoinz-cli.exe stop
bitcoinzd.exe

Wait 5-10 minutes.

Check Connections:
Run:

bitcoinz-cli.exe getpeerinfo

You should only see peers with .onion addresses (e.g., 7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989).

Note: If you don’t see any peers, it means there are no other .onion nodes available. In this case, you can disable onlynet=onion to connect to ipv4/ipv6 peers via Tor:

#onlynet=onion

Then restart BitcoinZ.

Step 5: Use Your BitcoinZ Wallet
Once connected to the .onion node, you can use your BitcoinZ wallet as usual:
Send and receive transactions.

Check your balance with:

bitcoinz-cli.exe getbalance

Ensure Tor remains running (Tor Browser or manual Tor) while using BitcoinZ.

Troubleshooting
Tor Won’t Connect:
Verify that Tor is running (Bootstrapped 100% (done) if using manual Tor, or check check.torproject.org if using Tor Browser).

If Tor doesn’t connect, try restarting Tor or checking your internet connection.

BitcoinZ Doesn’t Connect to the .onion Node:
Ensure addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989 is in bitcoinz.conf.

Check BitcoinZ logs for errors:

notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\debug.log

If you don’t see the .onion node in getpeerinfo, the node might be temporarily offline. In this case, disable onlynet=onion to connect to other peers via Tor.

Syncing Issues:
If BitcoinZ isn’t syncing, check if you’re connected to peers:

bitcoinz-cli.exe getnetworkinfo

Look for "connections". If the value is 0, try adding other nodes or disable onlynet=onion.

