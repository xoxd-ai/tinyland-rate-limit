# Changelog

## 1.0.0 (2026-10-14)

Major release for the 2026-10-08 estate uplift (RU1, RU10). The runtime API is
unchanged: every export of 0.3.0 (rate limiting, CSRF and the scope/subject
throttle with memory, Postgres and composite stores) keeps its name and
behaviour, and `src/` is byte-identical to v0.3.0. The toolchain and the
distribution path change.

### Changed

- TypeScript 7.0.2, vitest 5.0.3 and vite 8.3.3 are pinned exactly (RU1, RU5,
  RU13). `@types/node` moves to `^22`.
- tsconfig lists `"types": ["node"]`, which TypeScript 6+ no longer infers.
- Bazel: aspect_rules_ts 3.10.1 with the `typescript` extension at 7.0.2
  (`ts_project` keeps `transpiler = "tsc"`). The npm_translate_lock repo is
  renamed to `tummycrypt_tinyland_rate_limit_npm`, so it cannot collide with a
  root module's `npm` repo. A `//:typecheck` target is added, and
  `package.json` is in the `vitest_test` data.
- Distribution is Bazel only (RU6, RU8): the package is `private`, and the
  publish workflow, the tag trigger, `publishConfig` and `prepublishOnly` are
  removed. CI validates only (`dry_run: true`, `npm_publish_mode: disabled`).
- Repository, homepage and bugs URLs point at xoxd-ai.

### Migration

1. Stop installing `@tummycrypt/tinyland-rate-limit` from npmjs or GitHub
   Packages. 0.x there is frozen and will be marked deprecated.
2. Re-pin: `bazel_dep(name = "tummycrypt_tinyland_rate_limit", version = "1.0.0")`
   from xoxd-ai/bazel-registry, and link it with
   `npm_link_package(name = "node_modules/@tummycrypt/tinyland-rate-limit", src = "@tummycrypt_tinyland_rate_limit//:pkg")`.
3. Imports are unchanged. Consumers still on the tinyland.dev `packages/`
   copy (0.2.2) also gain the throttle exports (`createThrottle`,
   `createMemoryThrottleStore`, `createPgThrottleStore`,
   `createCompositeThrottleStore`, `pgThrottleTableDdl` and their types).
4. `pg` stays an optional peer (`^8.11.0`), needed only for
   `createPgThrottleStore`.

## 0.3.0

- Scope/subject sliding-window throttle with a pluggable store (memory,
  Postgres, composite).

## 0.2.2

- Roll forward published package versions so the next release re-establishes
  npm artifact truth for the current repo contents.

## 0.2.1

- Strip .js.map sourcemaps from published packages and resolve `workspace:*`
  dependencies to real version ranges.
