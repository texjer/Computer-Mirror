# mac-mirror (mirror.texs.org)

A static page in `public/`, served by the assets-only Cloudflare Worker `mirror`
(`wrangler.jsonc`) and, until the cutover, by the Vercel project `mac-mirror`.

Deploy = `/commitpush` (`.claude/commitpush.local.md`): pushing `main` has Workers
Builds run `npx wrangler deploy`, and Vercel builds the same push. Don't run
`npm run deploy` on top of a push. Local preview: `npm run dev` → http://armx2.local:8790.
