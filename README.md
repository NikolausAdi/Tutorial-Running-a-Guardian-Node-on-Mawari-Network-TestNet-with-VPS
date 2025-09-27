# Tutorial Running a Guardian Node on Mawari Network TestNet with VPS
> This tutorial is based on the official documentation from Mawari: *Operating the Guardian Node Testnet*,
> Source: [https://docs.mawari.net/decentralized-infrastructure-offering-dio/operating-the-guardian-node-testnet](https://docs.mawari.net/decentralized-infrastructure-offering-dio/operating-the-guardian-node-testnet)

📌 Requirements
- VPS / Server (recommended: Ubuntu 22.04/24.04, 4+ vCPU, 8GB+ RAM).
- SSH access (e.g., PuTTY).
- A testnet wallet (e.g., MetaMask).


## 1. Setup Wallet, Tokens, and Guardian NFT

1. **Create a Wallet**
   * If you don’t have one yet, create a wallet (e.g., [MetaMask](https://metamask.io/)).
   * Store your **private key & seed phrase** safely.

2. **Connect Wallet to Mawari TestNet**
   * Open [https://testnet.mawari.net/](https://testnet.mawari.net/).
   * Click **Connect Wallet** → choose your wallet → switch to **Mawari Network TestNet**.

3. **Get TestNet Tokens (MAWARI)**
   * Open [https://hub.testnet.mawari.net](https://hub.testnet.mawari.net).
   * Paste your wallet address → click **Request Token**.
   * You can request up to 2 tokens per wallet.

4. **Mint Guardian NFT**
   * Go back to [https://testnet.mawari.net/](https://testnet.mawari.net/).
   * Mint up to **3 Guardian NFTs**.


## 2. Install Docker on VPS Ubuntu

Login to your VPS via SSH (e.g., PuTTY), then run:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git
```

Install Docker:

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

⚠️ Logout and log back in to apply Docker group permissions.


## 3. Setup Environment Variables

Replace `0xYOUR_WALLET_ADDRESS` with **your main wallet address** (the one holding Guardian NFTs).

```bash
export MNTESTNET_IMAGE=us-east4-docker.pkg.dev/mawarinetwork-dev/mwr-net-d-car-uses4-public-docker-registry-e62e/mawari-node:latest
export OWNER_ADDRESS=0xYOUR_WALLET_ADDRESS
```


## 4. Run the Guardian Node

```bash
mkdir -p ~/mawari && docker run --pull always -v ~/mawari:/app/cache -e OWNERS_ALLOWLIST=$OWNER_ADDRESS $MNTESTNET_IMAGE
```

If successful, you should see logs like:

```
[INFO] Using burner wallet {"address": "<your_burner_wallet>"}
```

📌 Save the **burner wallet address**, it will be used for delegation.


## 5. Transfer Tokens to Burner Wallet

* Send **1 MAWARI token** from your main wallet to the **burner wallet**.
* If you only have 1 token, request an additional token directly to your burner wallet via:
  👉 [https://hub.testnet.mawari.net](https://hub.testnet.mawari.net)


## 6. Activate the Guardian Node

1. Open [https://app.testnet.mawari.net/](https://app.testnet.mawari.net/).
2. Connect your main wallet.
3. Select your Guardian NFT → click **Delegate**.
4. Enter your **burner wallet address** → click **Delegate** again.
5. Sign the transaction in your wallet.


## 7. Verify Node Status

* Check the VPS logs. If delegation is successful, you should see:

```
[DEBUG] received delegation offers count {"delegation offers": "1"}
[INFO] delegation offer accepted {"hash": "..."}
```

* Or verify via the Guardian Dashboard → your node should appear as **Running** ✅.


## 8. Run Node in Background (Optional)

To keep the node running after closing PuTTY:

```bash
sudo apt install -y screen
screen -S mawari
```

Run the node inside the screen session, then detach with:

```
CTRL + A + D
```

To reattach later:

```bash
screen -r mawari
```


## 🎉 Done!

Your Guardian Node is now running on the Mawari Testnet using your **main wallet address**.

