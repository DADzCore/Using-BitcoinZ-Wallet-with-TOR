# 📡 BitcoinZ Wallet Setup Guide: Connect via Tor to an `.onion` Node for Enhanced Privacy

Welcome to this step-by-step guide to set up your BitcoinZ wallet to connect to the network via Tor and join an `.onion` node for maximum anonymity! 🌐 By following this tutorial, you’ll enhance your privacy while contributing to a more secure BitcoinZ network. This guide is designed for Windows users, but the concepts can be adapted for other platforms.

**Why Use Tor and an `.onion` Node?**  
Using Tor hides your IP address, and connecting to an `.onion` node ensures your traffic stays within the Tor network, preventing leaks and improving anonymity. Let’s get started! 🚀

---

## 📋 Prerequisites
Before we begin, ensure you have:
- A Windows computer (adaptable for Linux/macOS).
- An internet connection.
- The BitcoinZ wallet (version 2.1.0 or later recommended).
- Tor (via Tor Browser or manual installation).

**Goal**: Configure your BitcoinZ wallet to connect via Tor and specifically to the `.onion` node: `7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989`.

---

## 🛠️ Step 1: Install Tor for Anonymous Connections

Tor is a powerful tool for anonymizing your network traffic. We’ll use it to route your BitcoinZ wallet connections securely.

### Option 1: Use Tor Browser (Recommended for Beginners)
1. **Download Tor Browser**:
   - Get it from the official Tor Project website: [Download Tor Browser](https://www.torproject.org/download/).
2. **Install Tor Browser**:
   - Run the installer and follow the prompts (e.g., install to `C:\Users\<your_user>\Downloads\tor-browser`).
3. **Launch Tor Browser**:
   - Open Tor Browser and ensure it connects to the Tor network.
   - Visit [check.torproject.org](https://check.torproject.org/) to confirm you’re connected to Tor.
4. **Keep Tor Browser Running**:
   - Tor Browser uses port `9050` for SOCKS and port `9151` for the `ControlPort`. Leave it open during this tutorial.

### Option 2: Install Tor Manually (For Advanced Users)
1. **Download the Tor Expert Bundle**:
   - Get the latest version from the Tor Project: [Download Tor Expert Bundle](https://www.torproject.org/download/tor/) (e.g., `tor-expert-bundle-windows-x86_64-14.5a5`).
2. **Extract the Bundle**:
   - Extract to a folder, e.g., `C:\Users\<your_user>\Downloads\tor-expert-bundle-windows-x86_64-14.5a5`.
3. **Create a `torrc` File**:
   - Open a command prompt:
     ```
     cd C:\Users\<your_user>\Downloads\tor-expert-bundle-windows-x86_64-14.5a5\tor
     notepad torrc
     ```
   - Add these lines:
     ```
     SocksPort 9050
     ControlPort 9051
     ```
   - Save the file (ensure it’s named `torrc`, not `torrc.txt`). Else run on command prompt :
   ```
     ren torrc.txt torrc
     ```
4. **Launch Tor**:
   - Run:
     ```
     tor.exe -f torrc
     ```
   - Wait for Tor to connect. You’ll see:
     ```
     Bootstrapped 0% (starting)
     ...
     Bootstrapped 100% (done)
     ```
   - This may take 1-2 minutes. Keep the window open.

---

## 💻 Step 2: Install the BitcoinZ Wallet

Let’s get the BitcoinZ wallet up and running!

1. **Download the BitcoinZ Wallet**:
   - Grab the latest version from the official GitHub: [BitcoinZ Releases](https://github.com/btcz/bitcoinz/releases).
   - For Windows, download `bitcoinz-windows-2.1.0.zip`.
2. **Extract the Wallet**:
   - Extract to a folder, e.g., `C:\Users\<your_user>\AppData\Local\BTCZCommunity\BTCZWallet\Data`.
3. **Verify Files**:
   - Ensure `bitcoinzd.exe` and `bitcoinz-cli.exe` are in the folder.

---

## 🔒 Step 3: Configure BitcoinZ to Connect via Tor

Now, let’s configure your wallet to use Tor and connect to the `.onion` node.

1. **Create or Edit `bitcoinz.conf`**:
   - Open a command prompt:
     ```
     notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\bitcoinz.conf
     ```
   - If the file doesn’t exist, Notepad will create it.
   - Add the following configuration:
     ```
     proxy=127.0.0.1:9050
     torcontrol=127.0.0.1:9051
     listen=1
     bind=127.0.0.1
     listenonion=0
     torstartupdelay=120
     addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989
     ```
     - `proxy=127.0.0.1:9050`: Routes traffic through Tor (Tor’s SOCKS port).
     - `torcontrol=127.0.0.1:9051`: Uses Tor’s `ControlPort` (9151 for Tor Browser, 9051 for manual Tor).
     - `listen=1` and `bind=127.0.0.1`: Allows BitcoinZ to listen on `127.0.0.1:1989`.
     - `listenonion=0`: Prevents BitcoinZ from creating its own onion service (avoids errors).
     - `torstartupdelay=120`: Gives Tor time to start.
     - `addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989`: Connects to the `.onion` node.
   - Save the file.

2. **Launch BitcoinZ**:
   - Ensure Tor is running (Tor Browser or manual Tor).
   - Start BitcoinZ:
     ```
     cd C:\Users\<your_user>\AppData\Local\BTCZCommunity\BTCZWallet\Data
     bitcoinzd.exe
     ```
   - Wait 5-10 minutes for BitcoinZ to sync with the blockchain.

3. **Verify Tor Usage**:
   - Run:
     ```
     bitcoinz-cli.exe getnetworkinfo
     ```
   - Look for the `"networks"` section. You should see:
     ```json
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
     ```
   - This confirms BitcoinZ is using Tor for all connections.

4. **Verify Connection to the `.onion` Node**:
   - Run:
     ```
     bitcoinz-cli.exe getpeerinfo
     ```
   - Look for a peer with:
     ```
     "addr": "7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989"
     ```
   - If present, you’re successfully connected to the `.onion` node!

---

## 🌐 Step 4: (Optional) Switch to an Onion-Only Setup

For maximum privacy, you can configure your node to connect only to `.onion` peers, avoiding `ipv4`/`ipv6` connections.

1. **Edit `bitcoinz.conf`**:
   - Open:
     ```
     notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\bitcoinz.conf
     ```
   - Add or uncomment:
     ```
     onlynet=onion
     ```
   - Save the file.

2. **Restart BitcoinZ**:

    Run   bitcoinz-cli.exe stop
    Run   bitcoinzd.exe

######
Wait 5-10 minutes.

3. **Check Connections**:
- Run:
  ```
  bitcoinz-cli.exe getpeerinfo
  ```
- You should only see `.onion` peers (e.g., `7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989`).
- **Note**: If no peers are listed, there may not be other `.onion` nodes available. You can disable `onlynet=onion` to connect to `ipv4`/`ipv6` peers via Tor:
  ```
  #onlynet=onion
  ```
  Then restart BitcoinZ.

---

## 💰 Step 5: Use Your BitcoinZ Wallet

You’re all set! You can now use your BitcoinZ wallet with enhanced privacy:
- Send and receive transactions.
- Check your balance:

Run   bitcoinz-cli.exe getbalance

######

- Keep Tor running (Tor Browser or manual Tor) while using BitcoinZ.

---

## 🛠️ Troubleshooting

- **Tor Won’t Connect**:
- Ensure Tor is running (`Bootstrapped 100% (done)` for manual Tor, or check [check.torproject.org](https://check.torproject.org/) for Tor Browser).
- If Tor doesn’t connect, restart Tor or check your internet connection.
- **BitcoinZ Doesn’t Connect to the `.onion` Node**:
- Verify `addnode=7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989` is in `bitcoinz.conf`.
- Check BitcoinZ logs for errors:
  ```
  notepad C:\Users\<your_user>\AppData\Roaming\BitcoinZ\debug.log
  ```
- If the `.onion` node isn’t in `getpeerinfo`, it might be offline. Disable `onlynet=onion` to connect to other peers via Tor.
- **Syncing Issues**:
- Check if you’re connected to peers:
  ```
  bitcoinz-cli.exe getnetworkinfo
  ```
  Look for `"connections"`. If it’s 0, add other nodes or disable `onlynet=onion`.

---

## 🎉 Conclusion

Congratulations! You’ve configured your BitcoinZ wallet to connect via Tor and to the `.onion` node `7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989`. This setup enhances your privacy and strengthens the BitcoinZ network. 🌍

Join the BitcoinZ community on [Discord](https://discord.com/invite/bitcoinz), [Telegram](https://t.me/bitcoinzcommunity), or [GitHub](https://github.com/btcz/bitcoinz) to share your experience or get help. Let’s build a more private BitcoinZ network together! 💪

---

*Tutorial created by the BitcoinZ community. Special thanks to the operator of the `.onion` node `7fdkg25kqtjfogktfjy5darpd5ff6ty7n34ol575yb4dp7dfzc4sdkid.onion:1989` for providing this anonymous connection point.*

ENJOY !
