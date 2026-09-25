# adapter-cloudflare keeps Vitest from exiting

Minimal reproduction: `@sveltejs/adapter-cloudflare` starts wrangler's
`getPlatformProxy()` from its Vite `configureServer` hook and never disposes
it. Under Vitest, the tests pass and then `vitest --run` hangs for 10 seconds
before printing `close timed out`.

```sh
pnpm install
pnpm test
```

Expected: exits right after `1 passed`.
Actual: passes, then waits 10s and prints

```
close timed out after 10000ms
Tests closed successfully but something prevents 2 Vite servers from exiting
```

Removing `adapter: adapter()` from `vite.config.ts` makes the hang go away.

Versions: `@sveltejs/kit` 3.0.0-next.29, `@sveltejs/adapter-cloudflare`
8.0.0-next.7, `vite` 8.3.1, `vitest` 5.0.2, `wrangler` 4.140.0, Node 24, pnpm 12.6.0
(macOS).
