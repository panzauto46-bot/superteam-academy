<p align="center">
  <img src="https://img.shields.io/badge/Solana-Devnet-14F195?style=for-the-badge&logo=solana&logoColor=white" alt="Solana Devnet" />
  <img src="https://img.shields.io/badge/Next.js-16-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js 16" />
  <img src="https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript 5" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-4-38BDF8?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind CSS 4" />
  <img src="https://img.shields.io/badge/Anchor-0.31+-E8D44D?style=for-the-badge" alt="Anchor 0.31+" />
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="MIT License" />
</p>

<h1 align="center">Superteam Academy</h1>

<p align="center">
  <strong>The Ultimate Interactive Hub for Solana Native Builders</strong>
</p>

<p align="center">
  A fully gamified, decentralized learning platform for mastering Solana blockchain development.<br/>
  Write real Solana programs in your browser, earn soulbound XP tokens on-chain, collect Metaplex Core NFT credentials, and compete on the leaderboard.
</p>

<p align="center">
  <a href="#-features">Features</a> &bull;
  <a href="#-architecture">Architecture</a> &bull;
  <a href="#-tech-stack">Tech Stack</a> &bull;
  <a href="#-getting-started">Getting Started</a> &bull;
  <a href="#-monorepo-structure">Monorepo Structure</a> &bull;
  <a href="#-on-chain-program">On-Chain Program</a> &bull;
  <a href="#-roadmap">Roadmap</a>
</p>

---

## Highlights

| Metric | Value |
|--------|-------|
| **Courses** | 5 interactive learning paths |
| **Lessons** | 15 hands-on coding challenges |
| **Languages** | EN, PT-BR, ES |
| **On-Chain Instructions** | 16 Anchor instructions |
| **PDA Account Types** | 6 (Config, Course, Enrollment, MinterRole, AchievementType, AchievementReceipt) |
| **Test Coverage** | 62 TypeScript + 77 Rust unit tests |
| **XP System** | Soulbound Token-2022 (NonTransferable + PermanentDelegate) |
| **Credentials** | Metaplex Core NFTs (soulbound, upgradeable) |

---

## Features

### Learning Experience
- **In-Browser Code Editor** -- Write and test Rust, TypeScript, and Anchor code with Monaco Editor (VS Code engine)
- **Split-Screen Learning** -- Left panel for lesson content (Markdown), right panel for live code editor
- **Course Detail Pages** -- Rich pages with overview, learning objectives, prerequisites, and full syllabus
- **Progressive Difficulty** -- Beginner to Advanced tracks covering Solana 101 through Metaplex NFTs

### Web3 Gamification
- **Soulbound XP Tokens** -- Earn XP on-chain via Token-2022 (NonTransferable, no self-burn)
- **Level System** -- Dynamic level formula: `Level = floor(sqrt(xp / 100))`
- **Daily Streaks** -- 28-day streak calendar tracking learning consistency
- **Achievement Badges** -- Unlock achievements for milestones (first lesson, course completion, streaks)
- **Global Leaderboard** -- Compete with other builders, ranked by on-chain XP balance

### On-Chain Credentials
- **Metaplex Core NFTs** -- Soulbound credential NFTs issued on course completion
- **Upgradeable Attributes** -- Credentials can be upgraded as learners progress
- **Helius DAS API** -- Real-time on-chain certificate reading and display

### Authentication & Access
- **Multi-Auth System** -- Phantom, Solflare (Solana wallets) + Google, GitHub (OAuth via NextAuth.js v5)
- **Unified Auth Context** -- Seamless experience across wallet and social authentication
- **Dark Mode Primary** -- Sleek dark-first design with Solana/Superteam green-neon branding

### Internationalization
- **3 Languages** -- Full i18n support (100+ translation keys) for English, Portuguese (BR), and Spanish
- **Extensible** -- Easy to add new languages via `translations.ts`

---

## Architecture

### Monorepo Overview

```
superteam-academy/
├── app/                         <-- Next.js 16 Frontend (you are here)
├── onchain-academy/             <-- Anchor On-Chain Program (Rust)
├── docs/                        <-- Technical Documentation
│   ├── SPEC.md                  <-- Program Specification (v3.0)
│   ├── ARCHITECTURE.md          <-- System Diagrams & Account Maps
│   └── INTEGRATION.md           <-- Frontend Integration Guide
├── scripts/                     <-- Helper Scripts
├── wallets/                     <-- Keypairs (gitignored)
└── CLAUDE.md                    <-- AI Agent Configuration
```

### Frontend Architecture (app/)

```
src/
├── app/                          # Next.js App Router
│   ├── (main)/                   # Route group with shared Navbar + Footer
│   │   ├── page.tsx              # Home / Landing Page
│   │   ├── courses/
│   │   │   ├── page.tsx          # Course Catalog Grid
│   │   │   └── [slug]/page.tsx   # Course Detail Page
│   │   ├── dashboard/page.tsx    # Builder Dashboard (XP, streaks, certs)
│   │   ├── leaderboard/page.tsx  # Global Leaderboard
│   │   └── profile/page.tsx      # User Profile
│   ├── lesson/[courseId]/        # Split-screen Lesson View (no footer)
│   ├── api/
│   │   ├── auth/[...nextauth]/   # NextAuth.js API Routes
│   │   └── nfts/[wallet]/        # Helius DAS API Proxy
│   ├── layout.tsx                # Root Layout (dark mode default)
│   ├── providers.tsx             # All Context Providers
│   └── globals.css               # Global Styles + shadcn/ui theme
├── components/
│   ├── layout/                   # Navbar, Footer
│   ├── shared/                   # AuthModal, CodeEditor, Hero
│   └── ui/                       # shadcn/ui components (badge, button, dialog, etc.)
├── contexts/
│   ├── AuthContext.tsx            # Unified auth + game state (XP, level, streaks)
│   ├── ServiceContext.tsx         # Service layer (enrollment, lesson completion)
│   ├── LanguageContext.tsx        # i18n context (EN, PT-BR, ES)
│   ├── ThemeContext.tsx           # Dark/Light theme (dark default)
│   └── WalletProvider.tsx        # Solana Wallet Adapter configuration
├── data/
│   └── courses.ts                # 5 courses, 15 lessons, 8 achievements
├── i18n/
│   └── translations.ts           # Multi-language strings (100+ keys)
├── hooks/
│   └── useNFTs.ts                # Custom hook for fetching on-chain cNFTs
├── services/
│   └── interfaces.ts             # Service interfaces (ready for Anchor integration)
├── lib/
│   ├── auth.ts                   # NextAuth.js configuration
│   └── utils.ts                  # Utility functions (cn, etc.)
└── types/
    └── next-auth.d.ts            # NextAuth type augmentations
```

### Data Flow

```
User Action
    |
    v
React Component (client)
    |
    v
ServiceContext (abstraction layer)
    |
    v
[Stub Implementation]  --->  [Future: Anchor Program via @coral-xyz/anchor]
    |                              |
    v                              v
AuthContext (local state)     Solana Blockchain (on-chain state)
    |                              |
    v                              v
UI Update                     Token-2022 XP + Metaplex Core NFTs
```

---

## Tech Stack

### Frontend

| Layer | Technology |
|-------|-----------|
| **Framework** | Next.js 16 (App Router, React 19) |
| **Language** | TypeScript 5 (strict mode) |
| **Styling** | Tailwind CSS 4 + shadcn/ui (New York style) |
| **Code Editor** | Monaco Editor (`@monaco-editor/react`) |
| **Markdown** | react-markdown |
| **Authentication** | NextAuth.js v5 (Google, GitHub) + Solana Wallet Adapter |
| **Blockchain** | @solana/web3.js + Helius DAS API |
| **Icons** | Lucide React |
| **UI Primitives** | Radix UI (via shadcn/ui) |
| **Error Monitoring** | Sentry (client + server + edge) |
| **Analytics** | Google Analytics 4 |

### On-Chain Program

| Layer | Technology |
|-------|-----------|
| **Framework** | Anchor 0.31+ |
| **Language** | Rust 1.82+ |
| **XP Tokens** | Token-2022 (NonTransferable + PermanentDelegate) |
| **Credentials** | Metaplex Core NFTs (soulbound via PermanentFreezeDelegate) |
| **Testing** | Mollusk, LiteSVM, ts-mocha, Chai |
| **RPC** | Helius (DAS API for credential queries + XP leaderboard) |

---

## On-Chain Program

The Anchor program (`onchain-academy/`) implements 16 instructions across 6 PDA account types:

### Instructions

| Category | Instructions |
|----------|-------------|
| **Platform** | `initialize`, `update_config` |
| **Courses** | `create_course`, `update_course` |
| **Enrollment** | `enroll`, `complete_lesson`, `finalize_course`, `close_enrollment` |
| **Credentials** | `issue_credential`, `upgrade_credential` |
| **Minters** | `register_minter`, `revoke_minter`, `reward_xp` |
| **Achievements** | `create_achievement_type`, `award_achievement`, `deactivate_achievement_type` |

### Account Structure

| Account | Description | Key Derivation |
|---------|-------------|---------------|
| **Config** | Singleton platform state | `["config"]` |
| **Course** | Course metadata (max 16) | `["course", course_id]` |
| **Enrollment** | Per-learner progress (lesson bitmap) | `["enrollment", learner, course]` |
| **MinterRole** | XP minter registration | `["minter", authority]` |
| **AchievementType** | Achievement definition | `["achievement_type", id]` |
| **AchievementReceipt** | Award proof (idempotency) | `["achievement_receipt", learner, achievement_type]` |

### Design Decisions

- **XP = soulbound Token-2022** -- NonTransferable + PermanentDelegate (no transfer, no self-burn)
- **Credentials = Metaplex Core NFTs** -- soulbound, wallet-visible, upgradeable attributes
- **No LearnerProfile PDA** -- XP balance queried via Token-2022 ATA
- **Lesson bitmap** -- Efficient on-chain tracking of lesson completion (u128)
- **Rotatable backend signer** -- Stored in Config, rotatable via `update_config`

---

## Courses

| # | Course | Level | Lessons | XP Reward |
|---|--------|-------|---------|-----------|
| 1 | Solana 101: Foundations | Beginner | 3 | 800 |
| 2 | Anchor Framework Essentials | Intermediate | 3 | 1,200 |
| 3 | Solana Web3.js Client | Beginner | 3 | 800 |
| 4 | SPL Token Mastery | Intermediate | 3 | 1,200 |
| 5 | NFTs with Metaplex | Advanced | 3 | 1,600 |

---

## Getting Started

### Prerequisites

- **Node.js** 18.17+
- **npm** 9+ (or yarn/pnpm)
- A Solana wallet extension (optional): [Phantom](https://phantom.app/) or [Solflare](https://solflare.com/)

### Installation

```bash
# Clone the monorepo
git clone https://github.com/solanabr/superteam-academy.git
cd superteam-academy/app

# Install dependencies
npm install

# Copy environment variables template
cp .env.local.example .env.local
```

### Environment Variables

Create a `.env.local` file:

```env
# NextAuth.js (Required)
NEXTAUTH_SECRET=your_random_secret_here    # generate: openssl rand -base64 32
NEXTAUTH_URL=http://localhost:3000

# Google OAuth (optional)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret

# GitHub OAuth (optional)
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret

# Solana (optional - defaults to devnet)
NEXT_PUBLIC_SOLANA_NETWORK=devnet
NEXT_PUBLIC_SOLANA_RPC_URL=https://api.devnet.solana.com

# Helius DAS API (optional - for on-chain cNFT certificates)
HELIUS_API_KEY=your_helius_api_key

# Google Analytics 4 (optional)
NEXT_PUBLIC_GA_MEASUREMENT_ID=G-XXXXXXXXXX

# Sentry Error Monitoring (optional)
NEXT_PUBLIC_SENTRY_DSN=https://example@o0.ingest.sentry.io/0
```

> **Note:** Only `NEXTAUTH_SECRET` and `NEXTAUTH_URL` are required. All other variables are optional -- features gracefully degrade when not configured. Wallet connection works without any environment variables.

### Development

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### Build & Production

```bash
npm run build
npm start
```

### On-Chain Program (Anchor)

```bash
cd ../onchain-academy

# Build program
anchor build

# Run tests
cargo test --manifest-path tests/rust/Cargo.toml   # 77 Rust unit tests
anchor test                                          # 62 TypeScript integration tests

# Deploy to devnet
anchor deploy --provider.cluster devnet
```

---

## Deployment

### Vercel (Frontend)

1. Push code to GitHub
2. Import on [Vercel Dashboard](https://vercel.com/new)
3. Set **Root Directory** to `app/`
4. Add environment variables in **Settings > Environment Variables**
5. Deploy -- Vercel auto-detects Next.js

> **OAuth Callback URLs:**
> - Google: `https://your-domain.vercel.app/api/auth/callback/google`
> - GitHub: `https://your-domain.vercel.app/api/auth/callback/github`

### Solana Program (Devnet)

```bash
cd onchain-academy
anchor build
anchor deploy --provider.cluster devnet --program-keypair wallets/program-keypair.json
```

Program ID: `3YchgRgR65gdRqgTZTM5qQXqtTZn5Kt2i6FPnZVu34Qb`

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.

---

<p align="center">
  Built with dedication for the Solana ecosystem<br/>
  <sub>Superteam Academy &copy; 2025-2026 | Superteam Brazil</sub>
</p>
