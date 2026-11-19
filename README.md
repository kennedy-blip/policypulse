# QuotaPulse

QuotaPulse is a full-stack API usage metering ledger, quota guardrail console and billing tier cost simulator. It combines a Vite + React + TypeScript dashboard with Vercel Serverless Functions in one deployable repository.

## Features

- Tenant usage board with quota progress, plan switching and unit-event emission
- Usage ledger with meter keys, unit deltas, estimated charges and timestamps
- Free, Builder, Scale and Enterprise tier comparison with volume-based cost projections
- Seed tenants: `acme-edge-prod`, `meru-farm-ai` and `nairobi-pay-sandbox`
- Local demo fallback when the Vercel API is not running
- Dark developer console built with Tailwind CSS v4

## Development

```bash
npm install
npm run dev
```

The Vite-only server serves the frontend and automatically uses the local demo ledger. To exercise the actual serverless endpoints locally, install the Vercel CLI and run:

```bash
npx vercel dev
```

## Production build

```bash
npm install
npm run build
```

## API

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/api/usage` | Tenant ledger, aggregate volume and recent events |
| POST | `/api/usage` | Record billable units; validates input and enforces hard caps |
| PATCH | `/api/usage` | Change a tenant's plan and included quota |
| DELETE | `/api/usage` | Reset the simulated billing cycle |
| GET | `/api/tiers` | Return plan definitions |
| POST | `/api/tiers` | Project cost for a monthly unit volume |

Example usage event:

```json
{"tenantId":"acme-edge-prod","meterKey":"vector.query","units":1000,"unitCost":0.000014}
```

## Deploy to Vercel

1. Import `kennedy-blip/policypulse` from GitHub in Vercel.
2. Keep the project root at the repository root.
3. Use the Vite framework preset. Vercel detects the `api/` serverless functions automatically.
4. Deploy; no environment variables are required for the demo.

## Important production note

This starter intentionally uses an in-memory demo ledger to keep the project self-contained and deterministic. Vercel Functions can run in separate, short-lived instances, so in-memory tenant changes are **not durable or globally consistent** in production. Before using real customer billing, replace `api/_data.ts` storage with a transactional database (for example Postgres), add tenant authentication and API-key hashing, idempotency keys, audit retention, rate limiting, and a verified billing reconciliation process. The displayed invoices are estimates; this project does not charge cards or process payments.

## Stack

- React 19, TypeScript, Vite
- Tailwind CSS v4 via `@tailwindcss/vite`
- Vercel Node.js serverless functions
- Lucide icons
