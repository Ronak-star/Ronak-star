<div align="center">

# 📄 ContractLens

**Upload a contract PDF. Get back the clauses worth a second look —**
**with a plain-English risk level and a page reference.**

[![CI](https://github.com/you/contractlens/actions/workflows/ci.yml/badge.svg)](https://github.com/you/contractlens/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Next.js](https://img.shields.io/badge/Next.js-App%20Router-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue?logo=typescript)
![Prisma](https://img.shields.io/badge/Prisma-ORM-2D3748?logo=prisma)
![Groq](https://img.shields.io/badge/Groq-Llama%203.3%2070B-orange)

**[🚀 Live Demo](#)** · **[📖 Architecture Docs](docs/architecture.md)** · **[🐛 Report a Bug](../../issues)**

</div>

---

## 📸 Screenshots

<div align="center">

| Landing Page | Dashboard |
|:---:|:---:|
| ![Landing page screenshot](docs/screenshots/landing.png) | ![Dashboard screenshot](docs/screenshots/dashboard.png) |

| Clause Analysis | Upload Flow |
|:---:|:---:|
| ![Clause analysis screenshot](docs/screenshots/analysis.png) | ![Upload flow screenshot](docs/screenshots/upload.png) |

</div>

> 💡 Add your own screenshots to `docs/screenshots/` and they'll render automatically above — the filenames just need to match.

---

## ✨ Features

- 🔐 **Email/password auth** — hashed passwords, rate-limited login via NextAuth
- 📤 **Upload any contract PDF** and get clause-level analysis in under a minute
- 🚦 **Every clause scored `high` / `medium` / `low` risk** with a plain-English reason and page hint
- 🔒 **Row-level authorization** — you only ever see your own contracts
- 🗑️ **Delete with confirmation**, with live analyzing/complete/error states
- 🔎 **SEO-ready landing page** with OpenGraph tags

---

## 🛠️ Tech Stack

| Layer | Tech |
|---|---|
| Framework | Next.js (App Router) |
| Language | TypeScript (strict mode) |
| Database / ORM | Prisma + SQLite (dev) / Postgres (prod) |
| Styling | Tailwind CSS |
| Auth | NextAuth (Credentials + JWT sessions) |
| AI | Groq API (Llama 3.3 70B) |
| Validation | Zod |

---

## ⚡ Quick Start

```bash
git clone https://github.com/you/contractlens && cd contractlens
cp .env.example .env        # then fill in GROQ_API_KEY and NEXTAUTH_SECRET
npm install
npm run db:migrate          # creates prisma/dev.db and applies the schema
npm run db:seed             # creates the demo account below
npm run dev                 # http://localhost:3000
```

Generate a `NEXTAUTH_SECRET` with:

```bash
openssl rand -base64 32
```

Get a **free** `GROQ_API_KEY` at [console.groq.com/keys](https://console.groq.com/keys) — no credit card required.

### 🔑 Demo Credentials

```
demo@demo.com / demo1234
```

---

## 🔧 Environment Variables

| Variable | Description |
|---|---|
| `DATABASE_URL` | `file:./dev.db` locally; a Postgres connection string in production |
| `NEXTAUTH_SECRET` | Random string used to sign session JWTs |
| `NEXTAUTH_URL` | Full URL of the app (`http://localhost:3000` locally) |
| `GROQ_API_KEY` | Your free Groq API key, used for clause extraction |

---

## 🏗️ Architecture

Short version:

```
User → Contract → Clause
```

- NextAuth credentials + JWT sessions
- PDF text extraction with `pdf-parse`
- Clause extraction via the Anthropic API, validated with Zod before it ever touches the database

📄 Full write-up, trade-offs, and diagrams: **[`docs/architecture.md`](docs/architecture.md)**

---

## ✅ Testing

This trial build doesn't ship automated tests yet — see [Roadmap](#-roadmap).

To verify manually:

```bash
npm run build   # confirms the production build and TypeScript strict-mode pass
```

**Manual checklist:**
1. Sign up
2. Upload a real contract PDF
3. Confirm clauses appear with sensible risk levels
4. Delete the contract
5. Confirm it's gone

---

## 🗺️ Roadmap

- [x] Auth, upload, AI clause extraction, dashboard
- [ ] Playwright e2e test of the upload → analyze → view flow
- [ ] Background job queue for analysis instead of a blocking request (better for large files)
- [ ] CSV export of a contract's clause list
- [ ] Team accounts / shared contract libraries

---

## ☁️ Deployment (Vercel)

1. Push this repo to GitHub, then **Import Project** at [vercel.com](https://vercel.com).
2. Provision a free Postgres instance (Neon or Supabase) and copy its connection string.
3. In `prisma/schema.prisma`, change `provider = "sqlite"` to `provider = "postgresql"`.
4. Add all four environment variables from the table above under **Settings → Environment Variables** (use the Postgres URL for `DATABASE_URL`, and your production domain for `NEXTAUTH_URL`).
5. Run `npx prisma migrate deploy` against the production database once (Vercel's build step or a one-off local run with the prod `DATABASE_URL`).
6. Every push to `main` auto-deploys; every PR gets a preview URL.

---

## 📄 License

MIT — see [LICENSE](LICENSE).
