<img width="767" height="354" alt="sharpy" src="https://github.com/user-attachments/assets/75892e8b-0991-4ef3-8ac9-d44d1ff72bb1" />

# Sharpy

Sharpy is an advanced split payment protocol built on Stellar Soroban. It enables creators to issue invoices that automatically distribute funds to multiple recipients according to configurable split rules. The protocol is designed for freelancers, DAOs, agencies, and any team that needs transparent, programmable, on-chain payment splitting.

## What Sharpy Does

Traditional payment splitting requires trust — someone collects funds and manually distributes them. Sharpy removes that trust requirement entirely. Once an invoice is created on-chain, the distribution logic is immutable and executes automatically when the invoice is fully funded.

Key capabilities:

- **Multi-recipient splits** — distribute a single payment to any number of recipients in one transaction
- **Split rules** — Fixed amounts, Percentage-based, or Tiered (threshold-triggered) splits evaluated at release time
- **Recurring invoices** — automatically create the next invoice in a series upon release, enabling subscriptions and retainers
- **Escrow protection** — hold funds for a configurable delay after full payment before releasing to recipients, providing a dispute window
- **Batch operations** — create or pay up to 10 invoices in a single transaction
- **Pool payments** — pay toward multiple invoices in one call
- **Audit log** — immutable on-chain record of every action taken on an invoice
- **Public verification** — anyone can verify invoice state without a wallet

## Deployments

| Network | Contract ID | Status |
|---------|-------------|--------|
| Testnet | `CAYTIFPD6RFWVHMK5SPPUUIWWAAANHKOJB6GOAJS5SR5MBKZMEY2UODZ` | Live |
| Mainnet | Coming soon | Pending |

- Testnet Explorer: https://stellar.expert/explorer/testnet/contract/CAYTIFPD6RFWVHMK5SPPUUIWWAAANHKOJB6GOAJS5SR5MBKZMEY2UODZ
- Live dApp: https://sharpy-sigma.vercel.app

## Repositories

| Repository | Description | Language |
|------------|-------------|----------|
| [sharpy-contracts](https://github.com/stellar-sharpy/sharpy-contracts) | Core Soroban smart contract | Rust |
| [sharpy-sdk](https://github.com/stellar-sharpy/sharpy-sdk) | TypeScript client SDK | TypeScript |
| [sharpy-app](https://github.com/stellar-sharpy/sharpy-app) | Next.js 14 frontend dApp | TypeScript |

## Technology Stack

Sharpy is built entirely on Stellar infrastructure:

- **Smart contract** — Rust, Soroban SDK v22, compiled to WASM and deployed on Stellar Soroban
- **Payments** — SEP-41 compatible token transfers (USDC on Stellar)
- **SDK** — TypeScript, `@stellar/stellar-sdk`, Freighter wallet integration
- **Frontend** — Next.js 14, Tailwind CSS, deployed on Vercel

## Why Stellar

Stellar's sub-cent transaction fees and 5-second finality make payment splitting economically viable at any scale. Splitting a $50 invoice between 5 recipients costs a fraction of a cent in fees. On Ethereum-based networks, the same operation would cost more in gas than the payment itself for small amounts.

Soroban's persistent storage and deterministic execution model make it well-suited for invoice state management. The contract stores full invoice history on-chain, making every payment and release publicly auditable.

## Split Rules

Split rules are evaluated per recipient at release time.

| Rule | Behaviour |
|------|-----------|
| `Fixed(amount)` | Recipient receives exactly this amount in stroops regardless of total funded |
| `Percentage(bps)` | Recipient receives `funded * bps / 10_000` (basis points) |
| `Tiered(threshold, bps)` | Recipient receives the percentage only if `funded > threshold`, otherwise receives 0 |

If no split rules are specified, funds are distributed proportionally based on each recipient's share of the invoice total.

## Quick Start

```typescript
import { SharpyClient, connectWallet, parseAmount, NETWORKS } from "@stellar-sharpy/sdk";

const publicKey = await connectWallet();
const client = new SharpyClient(NETWORKS.testnet);

const { invoiceId } = await client.createInvoice({
  creator: publicKey,
  recipients: [
    { address: "GABC...RECIPIENT1", amount: parseAmount("600") },
    { address: "GDEF...RECIPIENT2", amount: parseAmount("400") },
  ],
  token: "USDC_CONTRACT_ID",
  deadline: Math.floor(Date.now() / 1000) + 7 * 86400,
});

await client.pay(publicKey, invoiceId, parseAmount("1000"));
```

## Contributing

Each repository has its own `CONTRIBUTING.md` with setup instructions, code standards, and contribution guidelines. Issues and pull requests are welcome across all repositories.

- [Contract contributing guide](https://github.com/stellar-sharpy/sharpy-contracts/blob/main/CONTRIBUTING.md)
- [SDK contributing guide](https://github.com/stellar-sharpy/sharpy-sdk/blob/main/CONTRIBUTING.md)
- [App contributing guide](https://github.com/stellar-sharpy/sharpy-app/blob/main/CONTRIBUTING.md)

## Security

Do not report security vulnerabilities through public GitHub issues. Contact the maintainers directly. The contract does not hold admin keys on-chain — the admin is an externally owned account.

## License

All repositories are released under the MIT License.
