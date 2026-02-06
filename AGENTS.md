# AGENTS.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Common commands
Package manager: `pnpm` (see `pnpm-lock.yaml`).

### Install
- `pnpm install`

### Run (NestJS)
- Dev (watch): `pnpm run start:dev` (runs `nest start --watch`)
- Dev (debug + watch): `pnpm run start:debug`
- Run without watch: `pnpm run start`
- Prod (runs built output): `pnpm run start:prod`

### Build
- `pnpm run build` (outputs to `dist/`)

### Lint / format (Biome)
- Format: `pnpm run format`
- Lint (auto-fixes enabled): `pnpm run lint`
- Check (auto-fixes enabled): `pnpm run check`

Note: the Biome scripts here use `--write`, so they may modify files.

### Tests (Jest)
- Unit tests: `pnpm run test`
- Unit tests (watch): `pnpm run test:watch`
- Unit tests (single file): `pnpm run test -- --runTestsByPath src/app.controller.spec.ts`
- Unit tests (name/grep): `pnpm run test -- -t "<test name pattern>"`
- Coverage: `pnpm run test:cov`

E2E tests:
- `pnpm run test:e2e` (uses `test/jest-e2e.json`)
- E2E (single file): `pnpm run test:e2e -- --runTestsByPath test/app.e2e-spec.ts`

### Git hooks (Husky)
- `pre-commit`: runs `pnpm run check` (Biome check with `--write`).
- `commit-msg`: runs `pnpm exec commitlint --edit <commit-msg-file>` (rules in `commitlint.config.cjs`).

## High-level architecture
This is a minimal NestJS app scaffold.

- Entry point: `src/main.ts`
  - Bootstraps a Nest application from `AppModule` and listens on `process.env.PORT ?? 3000`.
- Composition root / DI container: `src/app.module.ts`
  - Currently wires `AppController` and `AppService`.
  - New feature modules should typically live under `src/<feature>/...` and be imported from `AppModule` (or from a higher-level module if the app grows).
- HTTP layer: `src/app.controller.ts`
  - Defines the `/` route and delegates to a service via Nest DI.
- Business logic: `src/app.service.ts`
  - Currently trivial (`getHello()`), but intended to hold injectable logic.

## Code rules

- ALWAYS reuse existing code and libraries when possible.
- All the files should be named in a way that is easy to understand and maintain.
- All the files should have max 200 lines of code.
- Use helpers or utils files to avoid code duplication and to keep the files small.
- All code should be written in TypeScript.
- All code should be written in a way that is easy to understand and maintain.
- All code should be written in a way that is easy to test.
- Follow existing patterns - Check neighboring files for conventions before writing new code.
- Make targeted edits - Update only parts that need to change. Avoid rewriting entire files.

## Testing layout
- Unit tests are colocated with source and use the `*.spec.ts` naming convention (see the `jest` config in `package.json`, with `rootDir: "src"`).
- E2E tests live in `test/` and use SuperTest against an in-memory Nest app created from `AppModule` (`test/app.e2e-spec.ts`, config in `test/jest-e2e.json`).
