# Dynamic Portfolio Dashboard

A full-stack stock portfolio dashboard developed as part of the Octa Byte AI Pvt Ltd Full-Stack Intern Assignment.

The application aims to display stock holdings, fetch market data, calculate portfolio performance, group holdings by sector, and periodically refresh market prices.

## Project Status

- [x] Set up a Turborepo monorepo
- [x] Use Bun as the package manager and runtime
- [x] Choose Next.js for the frontend
- [x] Choose Express.js for the backend
- [ ] Implement portfolio calculations and API endpoints
- [ ] Integrate market data providers
- [ ] Implement caching and periodic refresh
- [ ] Build the portfolio dashboard
- [ ] Configure CI with GitHub Actions
- [ ] Configure CD and deployment

## Objectives

- Display stock holdings in a responsive portfolio table.
- Calculate investment, present value, portfolio weight, and gain/loss.
- Group stocks by sector and display sector-level summaries.
- Fetch current market prices (CMP), P/E ratio, and latest earnings.
- Refresh market prices periodically.
- Handle provider failures, rate limits, and stale data.
- Apply automated testing and CI/CD practices.

## Technology Stack

| Area | Technology | Purpose |
|---|---|---|
| Monorepo | Turborepo | Manage applications and shared packages |
| Package manager | Bun | Dependency installation and scripts |
| Frontend | Next.js | Dashboard and routing |
| UI | React, TypeScript | Components and type safety |
| Styling | Tailwind CSS | Responsive design |
| Server state | TanStack Query (planned) | Caching, refetching, async states |
| Backend | Node.js, Express.js | REST API and business logic |
| Shared code | TypeScript package (planned) | Shared types and schemas |
| CI | GitHub Actions (planned) | Automated quality checks |
| CD | To be decided | Application deployment |

## Architecture

The project uses a monorepo to manage the frontend and backend in one repository.

```text
portfolio-dashboard/
├── apps/
│   ├── frontend/             # Next.js frontend
│   └── api/             # Express backend
├── packages/
│   └── shared/          # Shared types and schemas (planned)
├── .github/
│   └── workflows/       # CI/CD workflows (planned)
├── package.json
├── bun.lock
├── turbo.json
└── README.md
```

This is the intended structure. Adjust it to match the directories actually generated in the repository.

## Prerequisites

- Git
- Bun installed locally
- A GitHub repository for version control and GitHub Actions

Use the same Bun version locally and in CI.

## Setup Followed

### 1. Initialize the monorepo

Create a repository using Turborepo and Bun.

The repository contains separate applications for the frontend and backend, with the option to add shared packages.

### 2. Configure the frontend

Use Next.js with the App Router, TypeScript, and Tailwind CSS.

The frontend is responsible for:
- Rendering the portfolio dashboard.
- Displaying stock and sector summaries.
- Managing loading, error, and refresh states.
- Consuming the Express API.

### 3. Configure the backend

Use Node.js and Express.js for the API.

The backend is responsible for:
- Providing portfolio data through REST endpoints.
- Fetching and normalizing market data.
- Performing portfolio calculations.
- Handling provider failures and caching.

### 4. Install dependencies

Run from the repository root:

```bash
bun install
```

Commit the generated `bun.lock` file so local development and CI can use a consistent dependency lockfile.

### 5. Run the applications

The exact commands depend on the scripts configured in the root and application-level `package.json` files.

Once configured, the intended development command is:

```bash
bun run dev
```

This should start the applications through Turborepo.

## Portfolio Requirements

The dashboard will contain the following columns:

1. Particulars (stock name)
2. Purchase Price
3. Quantity
4. Investment
5. Portfolio weight (%)
6. NSE/BSE
7. Current Market Price (CMP)
8. Present Value
9. Gain/Loss
10. P/E Ratio
11. Latest Earnings

### Financial calculations

- Investment = Purchase Price × Quantity
- Present Value = CMP × Quantity
- Gain/Loss = Present Value − Investment
- Portfolio Weight (%) = Investment ÷ Total Investment × 100
- Return (%) = Gain/Loss ÷ Investment × 100, when investment is non-zero

Sector summaries will aggregate investment, present value, and gain/loss across holdings in each sector.

Gains will be displayed in green and losses in red.

## Market Data Strategy

The assignment specifies:
- Yahoo Finance for CMP.
- Google Finance for P/E ratio and latest earnings.

Neither source provides the public official API described in the assignment. Their unofficial integrations may be restricted or break when the underlying websites change.

The backend will isolate provider-specific integrations behind separate adapters. Responses will be normalized before being used by portfolio calculations.

The implementation will account for:
- API timeouts and errors.
- Rate limiting.
- Missing or invalid data.
- Cache expiry and stale data.
- Provider failures and fallback behavior.

Market data should not be presented as guaranteed real-time data if its freshness cannot be verified.

## Caching and Refresh Strategy

TanStack Query will manage frontend server state, including:
- Query caching.
- Loading and error states.
- Background refetching.
- Periodic refresh, initially targeting 15 seconds.

Backend caching will independently reduce requests to external providers.

A frontend refresh must not automatically trigger an external API call for every stock. The backend cache and provider request controls will determine when external data is fetched again.

## Git Branching Strategy

The project uses short-lived branches for individual changes.

| Branch | Purpose |
|---|---|
| `main` | Stable, reviewed code |
| `feat/*` | New features |
| `fix/*` | Bug fixes |
| `test/*` | Tests |
| `ci/*` | CI/CD configuration |
| `docs/*` | Documentation |
| `chore/*` | Tooling and maintenance |

Example branch names:

```text
feat/portfolio-api
feat/portfolio-dashboard
feat/market-data-cache
fix/portfolio-calculation
test/portfolio-calculations
ci/github-actions
docs/initial-readme
chore/turbo-setup
```

### Feature development workflow

```bash
git switch main
git pull origin main
git switch -c feat/portfolio-api

# Implement and test your changes
git add .
git commit -m "feat(api): add portfolio endpoint"
git push -u origin feat/portfolio-api
```

Open a pull request targeting `main`. Run CI and resolve failures before merging.

For this project, a separate permanent `develop` branch is optional rather than required.

## Continuous Integration

GitHub Actions will validate changes on pull requests and pushes to `main`.

Planned checks:

1. Check out the repository.
2. Set up the pinned Bun version.
3. Install dependencies using the lockfile.
4. Run linting.
5. Run TypeScript checks.
6. Run tests.
7. Build the applications.

The workflow will be configured after the corresponding scripts exist in the root and application-level package files.

## Continuous Deployment

CD will be configured after selecting the hosting platforms.

The planned deployment process includes:
- Separate production environment variables.
- Deployment triggered by verified changes to `main`.
- Application health checks.
- Post-deployment smoke tests.
- Deployment logs and rollback considerations.

## Documentation

Project documentation will be maintained throughout development.

Planned documents:

- `docs/architecture.md` — system architecture and request flows.
- `docs/technical-decisions.md` — decisions and trade-offs.
- `docs/development-log.md` — implementation progress and lessons learned.
- `docs/testing-strategy.md` — test coverage and testing approach.

## Testing Strategy

Tests will cover:

- Financial calculation formulas.
- Sector aggregation.
- API response validation.
- Missing and invalid market data.
- Upstream timeouts and failures.
- Cache behavior and refresh logic.

## Disclaimer

This project is for educational and informational purposes. Market data may be delayed, incomplete, or inaccurate. This dashboard is not investment advice.

## Assignment

Octa Byte AI Pvt Ltd — Dynamic Portfolio Dashboard with React.js/Next.js, TypeScript, Tailwind CSS, and Node.js.
