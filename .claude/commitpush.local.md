# /commitpush extras — mac-mirror

mirror.texs.org is served by the Worker `mirror` since 2026-10-17 (a zone route
`mirror.texs.org/*` on the proxied CNAME, see wrangler.jsonc). Pushing `main` has
Cloudflare Workers Builds deploy it; Vercel's GitHub integration still builds the idle
`mac-mirror` project until that project is deleted. Never run `npm run deploy` on top of
a push; that's a second Worker deploy over the one the push started.

Rollback: flip the mirror.texs.org CNAME back to DNS only in the texs.org zone; Vercel
answers again at once (the route only fires on proxied traffic).

## Workers Builds settings (Worker `mirror` → Settings → Builds)

- Repo `texjer/Computer-Mirror`, branch `main`.
- Build command: leave blank. There is nothing to build; `public/` is uploaded as is.
- Deploy command: `npx wrangler deploy`
- Preview builds: **off**. Only `main` is ever deployed, same as Vercel; a branch would
  just mint a second copy of the page.

## Lockfile: npm 10

The build installs with npm 10 (`npm ci`; the lock holds only wrangler). A lockfile
written by local npm 11 can drop optional `@emnapi/*` entries npm 10 insists on, and
the build fails before it starts. If `package.json` or `package-lock.json` is in this
push, run `npx npm@10 install --package-lock-only` first and include the rewritten lock.

## After the push: check the build

```bash
gh api repos/texjer/Computer-Mirror/commits/$(git rev-parse HEAD)/check-runs --jq '.check_runs[] | .name + " " + .status + " " + (.conclusion // "")'
```

Both Cloudflare and Vercel report here. It takes about a minute; check once. A failed
build leaves the old version live on that host. Report both results.
