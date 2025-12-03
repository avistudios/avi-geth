# Using AVI Coin with MetaMask

AVI Network is a Proof-of-Work Layer-1 chain whose native currency is
**AVI Coin (AVI)**.

- **Chain ID:** 369369  
- **Native currency:** AVI (displayed as **“AVI Coin”** in wallets)  
- **RPC:** `https://rpc.avicoin.org`  
- **Explorer:** `https://explorer.avicoin.org`  

Because the ticker **AVI** is already used by other projects on other
chains, some portfolio tools may show a different price feed for “AVI”.
Until AVI Network is widely indexed, treat **any price shown for “AVI”**
in your wallet as *not authoritative*.

The instructions below show how to add AVI Network to MetaMask and how
to integrate it in dApps.

---

## 1. Manual MetaMask Network Setup

In MetaMask:

1. Open the network selector at the top of the wallet.
2. Click **Add network** → **Add a network manually**.
3. Fill in the fields as follows:

   - **Network Name:**  
     `AVI Network`

   - **New RPC URL:**  
     `https://rpc.avicoin.org`

   - **Chain ID:**  
     `369369`

   - **Currency Symbol:**  
     `AVI Coin`  
     > This is only a display label. The canonical symbol in metadata is
     > still `AVI`, but using `AVI Coin` here helps distinguish it from
     > other “AVI” tokens on different chains.

   - **Block Explorer URL:**  
     `https://explorer.avicoin.org`

4. Click **Save**.

You should now see **AVI Network** as a selectable network in MetaMask.
Balances will show in **AVI Coin** and gas fees are paid in AVI Coin.

---

## 2. Programmatic Network Add (dApp integration)

dApps can prompt MetaMask (and other EIP-3085 compatible wallets) to add
AVI Network with:

```js
await window.ethereum.request({
  method: 'wallet_addEthereumChain',
  params: [
    {
      chainId: '0x5a2d9', // 369369 in hex
      chainName: 'AVI Network',
      nativeCurrency: {
        name: 'AVI Coin',
        symbol: 'AVI', // canonical symbol in metadata
        decimals: 18
      },
      rpcUrls: ['https://rpc.avicoin.org'],
      blockExplorerUrls: ['https://explorer.avicoin.org']
    }
  ]
});
```

Notes:

* The **metadata symbol** stays `AVI` (what Chainlist, Etherscan, SDKs
  will see).
* Wallets are free to display “AVI Coin” in UI; what matters for code is
  consistent symbol + ChainID.

---

## 3. Chain Metadata JSON (for Chainlist / tooling)

In `avi-geth/network/chain-metadata.json` (and optionally at
`https://avicoin.org/network/chain-metadata.json`), use:

```json
{
  "name": "AVI Network",
  "shortName": "avi",
  "chain": "AVI",
  "chainId": 369369,
  "networkId": 369369,
  "nativeCurrency": {
    "name": "AVI Coin",
    "symbol": "AVI",
    "decimals": 18
  },
  "rpc": [
    "https://rpc.avicoin.org"
  ],
  "explorers": [
    {
      "name": "AVI Block Explorer",
      "url": "https://explorer.avicoin.org",
      "standard": "EIP3091"
    }
  ],
  "infoURL": "https://avicoin.org",
  "status": "active"
}
```

This keeps the **canonical symbol** as `AVI` for infra/SDKs, while your
front-end and MetaMask UI can show **“AVI Coin”** as the human-friendly
name.

---

## 4. Ticker collision note (for docs)

You can keep this as-is with just the name tweak:

> **Ticker notice:**
> The ticker “AVI” is also used by other projects on other chains. Until
> major indexers and price oracles support AVI Network directly, any
> “AVI” price you see in wallets or portfolio apps may refer to a
> different asset. Always treat the on-chain balance on Chain ID 369369
> (AVI Network) as the source of truth.
