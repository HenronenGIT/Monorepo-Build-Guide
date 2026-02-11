# Monorepo Build Guide

This is a TypeScript monorepo managed with **pnpm workspaces** and **Turborepo**.

---

## Tech StackÏ

| Layer       | Technology                                  |
| ----------- | ------------------------------------------- |
| Frontend    | Next.js @latest (App Router), React @latest |
| UI          | Tailwind CSS @latest                        |
| Backend     | Express @latest, tsx @latest (dev runner)   |
| Database    | PostgreSQL, **Knex (query builder + migrations)** |
| State       | Zustand (client), Zod (validation)          |
| Testing     | Vitest (unit/integration), Playwright (e2e) |
| Linting     | ESLint + Prettier                           |
| Build       | Turborepo, TypeScript                       |
| Environment | pnpm, dotenv                                |


---

## Monorepo Structure

monorepo/
├── apps/
│ ├── web/ # Next.js frontend
│ │ ├── src/
│ │ │ ├── app/ # App Router (routes, layouts, pages)
│ │ │ ├── components/ # App-specific components
│ │ │ ├── lib/ # App-specific utilities
│ │ │ ├── hooks/ # Custom React hooks
│ │ │ ├── styles/ # Global styles
│ │ │ ├── types/ # App-local types (UI only, not shared)
│ │ │ └── env.ts # Zod-validated environment config
│ │ ├── public/
│ │ ├── next.config.ts
│ │ ├── tailwind.config.ts
│ │ ├── tsconfig.json
│ │ └── package.json
│ │
│ └── api/ # Express.js backend
│ ├── src/
│ │ ├── routes/ # Express route definitions (no business logic)
│ │ ├── controllers/ # HTTP layer (request/response handling only)
│ │ ├── services/ # Business logic layer
│ │ ├── repositories/ # Data access layer (Knex queries)
│ │ ├── middleware/ # Express middleware
│ │ ├── env.ts # Zod-validated environment config
│ │ └── index.ts # Express entry point
│ ├── tsconfig.json
│ └── package.json
│
├── packages/
│ ├── ui/ # Shared React component library
│ │ ├── src/
│ │ ├── package.json
│ │ └── tsconfig.json
│ │
│ ├── types/ # Shared TypeScript types & Zod schemas
│ │ ├── src/
│ │ ├── package.json
│ │ └── tsconfig.json
│ │
│ ├── utils/ # Shared utility functions
│ │ ├── src/
│ │ ├── package.json
│ │ └── tsconfig.json
│ │
│ ├── db/ # Shared Knex configuration + migrations
│ │ ├── migrations/
│ │ ├── seeds/
│ │ ├── src/
│ │ │ ├── client.ts # Knex instance
│ │ │ └── index.ts
│ │ ├── knexfile.ts
│ │ ├── package.json
│ │ └── tsconfig.json
│ │
│ ├── eslint-config/ # Shared ESLint configurations
│ │ └── package.json
│ │
│ └── typescript-config/ # Shared tsconfig base files
│ ├── base.json
│ ├── nextjs.json
│ ├── node.json
│ └── package.json
│
├── turbo.json # Turborepo pipeline config
├── pnpm-workspace.yaml # Workspace definition
├── package.json # Root package.json (private)
├── tsconfig.json # Optional root tsconfig
├── docker-compose.yml # Local services (DB, Redis, etc.)
├── .gitignore
└── .env.example

Internal packages are referenced by name (e.g. `@aggregate/shared`, `@aggregate/logger`).

---

## Key Commands

```bash
pnpm install              # Install all workspaces
pnpm run dev              # Start all apps in parallel (turbo)
pnpm run dev:web          # Start frontend only
pnpm run dev:api          # Start backend only
pnpm run build:web        # Production build (Next.js)
pnpm run lint             # ESLint across repo
pnpm run format:check     # Prettier check
```

## Conventions
- Zod schemas are prefixed with Z (e.g. ZEnvSchema).
- When inferring from a schema, use the infer keyword and prefix with T (e.g. TEnvSchema).
- Internal packages live in packages/ and are consumed via pnpm workspaces.
- Environment variables are validated at runtime using Zod in each app’s env.ts.
- App-local types (apps/web/src/types) are UI-only. Shared types and schemas must live in packages/types.

Backend layering rules:
- routes/ → routing only
- controllers/ → request/response handling only
- services/ → business logic
- repositories/ → database access (Knex)
