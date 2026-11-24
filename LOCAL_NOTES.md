# Local notes — nextjs-r7

Source: [`TailAdmin/free-nextjs-admin-dashboard`](https://github.com/TailAdmin/free-nextjs-admin-dashboard), version 2.5.2.

This is a fresh-history copy installed as `Zainpreneur/nextjs-r7` for the Zainpreneur Next.js admin reference set.

## Local changes

- Removed the `next/font/google` `Outfit` import from `src/app/[locale]/layout.tsx`; Google Fonts downloads time out in this environment.
- Replaced the `--font-outfit` token in `src/app/globals.css` with a system sans-serif fallback so existing `font-outfit` classes still work.
- Changed `postcss.config.js` from `export default` to `module.exports`; this repo has no `"type": "module"`, so Next was seeing an empty config and returning HTTP 500 on CSS imports.
- Removed nonessential lint/format dev dependencies (`eslint`, `eslint-config-next`, `prettier`, `prettier-plugin-tailwindcss`) that were not needed to run the reference and caused registry metadata fetch failures.
- Removed the upstream `package-lock.json`; `pnpm-lock.yaml` is the canonical lockfile for this copy.
- Kept `@svgr/webpack`, `@tailwindcss/forms`, type packages, PostCSS, Tailwind, and TypeScript because they are required by the app or its config.

## Install

```bash
NODE_OPTIONS='--dns-result-order=ipv4first --no-network-family-autoselection' \
pnpm install --registry https://registry.npmjs.org \
  --prefer-offline --network-concurrency 4 \
  --fetch-retries 20 --fetch-timeout 60000 --ignore-scripts
```

`NODE_OPTIONS=--dns-result-order=ipv4first` avoids IPv6 connection attempts, which fail in this environment.

`pnpm-lock.yaml` is the canonical lockfile for this copy.

## Run

```bash
./node_modules/.bin/next dev --webpack --hostname 0.0.0.0 --port 3005
```

`--webpack` is required because pnpm's symlinked `node_modules` breaks Turbopack here.

## Verification

Expected: `GET http://localhost:3005/` → HTTP 200.
