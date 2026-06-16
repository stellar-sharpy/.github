# Sharpy — Split Payments on Stellar

**Advanced on-chain payment splitting for the Stellar ecosystem.**

Sharpy lets you create invoices that automatically distribute funds to multiple recipients — with recurring schedules, escrow protection, and flexible split rules. Built on Stellar Soroban.

---

## Live

| | |
|---|---|
| **dApp** | [sharpy-sigma.vercel.app](https://sharpy-sigma.vercel.app) |
| **Testnet Contract** | [`CAYTIFPD6...`](https://stellar.expert/explorer/testnet/contract/CAYTIFPD6RFWVHMK5SPPUUIWWAAANHKOJB6GOAJS5SR5MBKZMEY2UODZ) |

---

## Repositories

| Repo | Description |
|------|-------------|
| [sharpy-contracts](https://github.com/stellar-sharpy/sharpy-contracts) | Soroban smart contract — Rust |
| [sharpy-sdk](https://github.com/stellar-sharpy/sharpy-sdk) | TypeScript SDK |
| [sharpy-app](https://github.com/stellar-sharpy/sharpy-app) | Next.js 14 frontend dApp |

---

## Features

- **Recurring invoices** — auto-generate next invoice on release
- **Escrow protection** — configurable delay before fund release
- **Batch operations** — up to 10 invoices in one transaction
- **Split rules** — Fixed, Percentage, and Tiered distributions

---

## Quick Start

```typescript
import { SharpyClient, connectWallet, parseAmount, NETWORKS } from "@stellar-sharpy/sdk";

const publicKey = await connectWallet();
const client = new SharpyClient(NETWORKS.testnet);

const { invoiceId } = await client.createInvoice({
  creator: publicKey,
  recipients: [{ address: "GABC...", amount: parseAmount("100") }],
  token: "USDC_CONTRACT_ID",
  deadline: Date.now() / 1000 + 7 * 86400,
});
```

---

MIT License · Built on [Stellar](https://stellar.org)
