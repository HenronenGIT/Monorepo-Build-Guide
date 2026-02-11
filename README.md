# Build Guide

This is a TypeScript monorepo managed with **pnpm workspaces** and **Turborepo**.

## Tech Stack

| Layer       | Technology                                  |
| ----------- | ------------------------------------------- |
| Frontend    | Next.js @latest (App Router), React @latest |
| UI          | Tailwind CSS @latest                        |
| Backend     | Express @latest, tsx @latest (dev runner)   |
| Database    | PostgreSQL, knex migrations                 |
| State       | Zustand (client), Zod (validation)          |
| Testing     | Vitest (unit/integration), Playwright (e2e) |
| Linting     | ESLint + Prettier                           |
| Build       | Turborepo, TypeScript                       |
| Environment | pnpm, dotenv                                |

## Monorepo Structure

```
monorepo/
├── apps/
│   ├── web/                  # Next.js frontend
│   │   ├── src/
│   │   │   ├── app/          # App Router (routes, layouts, pages)
│   │   │   ├── components/   # App-specific components
│   │   │   ├── lib/          # App-specific utilities
│   │   │   ├── hooks/        # Custom React hooks
│   │   │   ├── styles/       # Global styles
│   │   │   └── types/        # App-local types
│   │   ├── public/
│   │   ├── next.config.ts
│   │   ├── tailwind.config.ts
│   │   ├── tsconfig.json
│   │   └── package.json
│   │
│   └── server/               # Express.js backend
│       ├── src/
│       │   ├── routes/       # Route handlers
│       │   ├── controllers/  # Business logic
│       │   ├── middleware/   # Express middleware
│       │   ├── services/     # Service layer
│       │   ├── models/       # Data models
│       │   └── index.ts      # Express entry point
│       ├── tsconfig.json
│       └── package.json
│
├── packages/
│   ├── ui/                   # Shared React component library (shadcn, etc.)
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── types/                # Shared TypeScript types & interfaces
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── utils/                # Shared utility functions
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── db/                   # Shared database client (Prisma, Drizzle, etc.)
│   │   ├── prisma/
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── eslint-config/        # Shared ESLint configurations
│   │   └── package.json
│   │
│   └── typescript-config/    # Shared tsconfig base files
│       ├── base.json
│       ├── nextjs.json
│       ├── node.json
│       └── package.json
│
├── turbo.json                # Turborepo pipeline config
├── pnpm-workspace.yaml       # Workspace definition
├── package.json              # Root package.json (private)
├── tsconfig.json             # Optional root tsconfig
├── docker-compose.yml        # Local services (DB, Redis, etc.)
├── .gitignore
└── .env.example
```

Internal packages are referenced by name (e.g. `@aggregate/shared`, `@aggregate/logger`).

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

- Zod schemas are prefixed with `Z` (e.g. `ZEnvSchema`). When inferring from a schema, use the `infer` keyword and prefix with `T` (e.g. `TEnvSchema`).
- Internal packages live in `packages/` and are consumed via pnpm workspaces.
