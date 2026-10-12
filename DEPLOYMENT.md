# Deployment

Threadline builds to a fully static site (`next.config.ts` sets `output: 'export'`), so the `out/` folder can be hosted anywhere that serves static files.

## Branches and environments

| Branch    | Environment | Purpose                                   |
| --------- | ----------- | ----------------------------------------- |
| `main`    | Production  | Only merge from `staging` after QA passes |
| `staging` | Staging     | Integration branch; feature PRs target it |

Flow: feature branch → PR into `staging` → QA on the staging URL → PR `staging` → `main`.

## CI

`.github/workflows/ci.yml` runs on every push/PR to `main` and `staging`: `npm ci`, `npm run typecheck`, `npm test`, `npm run build`, and uploads `out/` as an artifact.

## Environment variables

Copy `.env.example` to `.env.local` for local work. In your host, set per environment (Production vs. Preview/Staging):

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

Both are optional — the app falls back to demo data when unset. Never add service-role keys; they are public at build time.

## Vercel (recommended)

1. Import `media-code-now/treadline` in Vercel. `vercel.json` sets the framework, build command and `out/` output directory.
2. Production branch: `main`. Add `staging` as a Preview branch (it is already enabled in `vercel.json`); optionally assign it a custom domain such as `staging.<your-domain>`.
3. Add the env vars above for Production and Preview separately so staging can point at a separate Supabase project.

## Other static hosts

Netlify, Cloudflare Pages, GitHub Pages, S3/CloudFront: build with Node 22.19+ (`.nvmrc`), run `npm ci && npm run build`, publish `out/`.

## Release checklist

- `npm run typecheck && npm test && npm run build` pass locally.
- Staging URL manually verified per the acceptance notes in `README.md`.
- `staging` merged to `main` via PR; tag the release (`git tag vX.Y.Z && git push --tags`).
