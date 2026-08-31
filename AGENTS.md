# AGENTS.md

Guidance for AI coding agents working in this repository. Provider-neutral; `CLAUDE.md` imports this file.

## Commands

Run from the repo root. `pnpm` is required (workspace protocol + pinned `packageManager: pnpm@9`); Node >= 18.

```sh
pnpm install
pnpm dev                          # turbo run dev — all apps, persistent, uncached
pnpm build                        # turbo run build
pnpm lint                         # turbo run lint
pnpm check-types                  # turbo run check-types
pnpm format                       # prettier --write "**/*.{ts,tsx,md}" — NOT a turbo task
```

Scope to one package with a turbo filter, using the `name` field from its `package.json`:

```sh
pnpm exec turbo dev --filter=landing
pnpm exec turbo lint --filter=@repo/ui
pnpm exec turbo build --filter=landing
```

`landing` dev serves on port 3000 (hardcoded in its `dev` script, not the Next default flag).

**There is no test framework in this repo.** No `test` task exists in `turbo.json` and no package defines a `test` script. Do not invent `pnpm test`; if tests are needed, adding a runner is a real decision to raise with the user first.

## Architecture

pnpm workspace (`apps/*`, `packages/*`) orchestrated by Turborepo.

| Package | Location | Role |
| --- | --- | --- |
| `landing` | `apps/landing` | Next.js 16 App Router app, React 19 |
| `@repo/ui` | `packages/ui` | Shared React components |
| `@repo/eslint-config` | `packages/eslint-config` | Three flat-config ESLint presets |
| `@repo/typescript-config` | `packages/typescript-config` | Three `tsconfig` bases |

The two config packages are consumed as `devDependencies` via `workspace:*` and exist only to be `extends`-ed / imported; they have no build or lint of their own.

### TypeScript

All three bases inherit `packages/typescript-config/base.json`: `strict`, plus **`noUncheckedIndexedAccess`** — array/record index access yields `T | undefined` and must be narrowed. This trips up otherwise-normal code and is the most common type error here.

- `nextjs.json` — apps: `moduleResolution: Bundler`, `jsx: preserve`, `noEmit`.
- `react-library.json` — `packages/ui`: `jsx: react-jsx`.
- `base.json` — `module`/`moduleResolution: NodeNext`.

`check-types` in `apps/landing` is `next typegen && tsc --noEmit`: the typegen step is required first because App Router route types are generated into `.next/types`. Running bare `tsc --noEmit` there can report spurious errors on a cold checkout.

## Conventions

- Add a workspace dependency with the `workspace:*` protocol, never a version range.
- New packages go in `apps/*` or `packages/*` (nothing else is globbed by `pnpm-workspace.yaml`), and need their own `eslint.config.js` selecting a preset and a `tsconfig.json` extending a base — turbo discovers tasks purely from each `package.json`.
- Run `pnpm format` rather than hand-formatting; Prettier is root-only and runs across `.ts`, `.tsx`, `.md`.
