# @tummycrypt/tinyland-rate-limit

Framework-agnostic rate limiting, CSRF protection and a scope/subject
sliding-window throttle (memory, Postgres or composite store) for
SvelteKit-style middleware.

## Use it

Distribution is Bazel only; nothing is published to npm. In your
`MODULE.bazel`, with `xoxd-ai/bazel-registry` in your registry list:

```starlark
bazel_dep(name = "tummycrypt_tinyland_rate_limit", version = "1.0.0")
```

and link it into your `node_modules`:

```starlark
npm_link_package(
    name = "node_modules/@tummycrypt/tinyland-rate-limit",
    src = "@tummycrypt_tinyland_rate_limit//:pkg",
)
```

`pg` is an optional peer, needed only for `createPgThrottleStore`.

## Develop

```bash
pnpm install --frozen-lockfile
pnpm typecheck && pnpm test && pnpm build
bazel build //:pkg && bazel test //...
```

See [CHANGELOG.md](CHANGELOG.md) for the 1.0.0 migration note.
