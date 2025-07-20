# fuze-governance-dao-lite

> **Snapshot strategies + SafeSnap module + off‑the‑shelf proposal templates**  
> Gives the FUZE community a gas‑free voting flow that can **execute on‑chain** via Safe multisig.

[![Snapshot](https://img.shields.io/badge/Snapshot.org-ready-8a63ff)](https://snapshot.org/)
[![SafeSnap](https://img.shields.io/badge/SafeSnap-module-green)](https://docs.gnosis-safe.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey.svg)](#license)

---

## ✨ What’s Included?

| Folder / Module               | Purpose                                                         |
| ----------------------------- | --------------------------------------------------------------- |
| `strategies/fuze-stake`       | Custom Snapshot strategy → vote weight = `staked FUZE + KPI‑bond`. |
| `safesnap/`                   | Safe module + Chainlink relayer config for automatic execution. |
| `templates/`                  | Markdown + JSON proposal blueprints (treasury spend, adapter add, KPI tweak). |
| `scripts/`                    | Hardhat tasks to deploy SafeSnap + link to FUZE Safe.           |
| `docs/`                       | Governance process docs, contributor flowchart.                |

---

## 🏗️ Quick Setup (Testnet)

### 1 Deploy SafeSnap

```bash
git clone https://github.com/fuze-ac/fuze-governance-dao-lite.git
cd fuze-governance-dao-lite
pnpm i
npx hardhat deploy:safesnap --network sepolia \
  --safe 0xYourSafe
````

Outputs SafeSnap module address; add it to the Safe owners list with `confirmations = 1`.

### 2 Create Snapshot Space

1. Go to **snapshot.org** → *Create space*.
2. Add Safe address + SafeSnap module in **Strategies → Execution**.
3. Paste the custom strategy name: `fuze-stake`.
4. Upload `strategies/fuze-stake/strategy.json` to the “Custom strategies” field.

### 3 First Proposal (template)

```bash
cp templates/treasury-disbursement.md my-proposal.md
# edit amount / recipient
```

Commit the Markdown + JSON to GitHub, share the link in Snapshot.

---

## 🌐 Strategy Details

```js
// strategies/fuze-stake/index.ts
weight = stakedFUZE + kpiBondStake;
minBalanceToVote = 1e18;      // 1 FUZE
```

* Reads Real‑Yield Vault share & KPI‑Bond escrows via multicall.
* Blocks sub‑second vote loan exploits (snapshot height lock).

---

## 🔒 Security & Safeguards

| Mechanism             | Detail                                       |
| --------------------- | -------------------------------------------- |
| **Timelock**          | SafeSnap execution delay (24 h default).     |
| **Quorum**            | 2 % of circulating FUZE stake.               |
| **Guardian veto**     | Multisig can cancel malicious tx before exec |
| **Chainlink relayer** | On‑chain outcome posted with L2 → L1 proof.  |

---

## 🧪 Tests

```bash
pnpm test                      # mocha + chai
```

Simulates malicious proposal → guardian veto → SafeSnap skip.

---

## 🛠️ Contributing

1. Fork → branch `feat/new-strategy-{name}`
2. `pnpm lint && pnpm test`
3. PR into `develop`.
4. Describe use‑case & risk in `/docs/strategies/README.md`.

All contributors sign the CLA.

---

## 📝 License

MIT © 2025 FUZE Foundation

