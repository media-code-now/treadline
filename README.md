# Threadline — foundation phase
Next.js, TypeScript, Tailwind CSS, shadcn/ui-style Button (Radix Slot + CVA), and Supabase client scaffolding.

## Run
`npm install`, then `npm run dev`. Production: `npm run build`. Static output is in `out/`.

## What works
All eight responsive sections; client filtering; keyword/discussion search; top-five filter; browser-local bookmarks and drafts; editing/deleting drafts; demo CSV export; community links; integration status screen.

## Verification
Run `npm run typecheck` and `npm run build`. Open the app, select Northstar, search onboarding, open the discussion, save a draft, refresh and check Drafts. Toggle a bookmark and refresh. Export a report and confirm the file explicitly labels simulated data. Test sidebar at mobile width.

## Disconnected
No live Reddit discovery, Google/SERP tracking, AI generation, posting, scheduled reports, team persistence, or authentication flow. All metrics/discussions are fictional demo data. Browser storage is per device/origin; no real client secrets should be entered in the demo. The Supabase migration has not been applied or tested against a live database.

## Supabase next phase
Copy `.env.example` to `.env.local`, add your project URL and public anon key, and apply `supabase/schema.sql` in a fresh Supabase project. Never expose service-role keys. `lib/supabase.ts` returns null until configured; UI intentionally continues using demo data until authenticated CRUD is implemented. Schema includes workspaces, membership, client projects, opportunities, drafts, rank observations and row-level access policies. Client portal roles and invitation flows remain future work.

## Phase 2 — Client Projects UI
Scope confirmed UI-only. All project profiles and the chosen client are saved in browser-local storage; authentication, server enforcement, Supabase CRUD and cross-client database isolation verification remain deferred. The existing foundation SQL is unchanged and is not proof of production client isolation.

The profile includes business name/URL/description, products/services, locations/language, brand spelling variations, competitors, keywords and sourced, manually reviewed business facts. Create and edit profiles, switch the workspace client, archive and restore projects. New projects start with no simulated results. Archiving hides a project from the active switcher and retains its data. Renaming retains the demo discussion and draft associations. Drafts show the corresponding business context and reviewed facts; modifying a fact or source clears its review checkbox.

Validation: required business context, unique names, HTTP/HTTPS website URLs, and source URLs for reviewed facts. Lists accept commas or newlines. Blank keywords, competitors and facts are allowed with useful empty states. Browser storage errors are displayed without pretending persistence succeeded.

Verification: Node 22.19+; `npm test` covers validation and list parsing. `npm run build` checks TypeScript and the static production export. Manual UI acceptance: create a project with all fields; switch to it; refresh and confirm selection/profile remain; edit its name; archive it and confirm it leaves the switcher; restore from Archived; add a fact and attempt review without a source to see validation; add a source and save; edit a sample opportunity and inspect its business context. Browser end-to-end QA has not been run in this environment.

## Phase 3 — Keywords and provider UI
UI-only scope. Keywords support all five categories, included/excluded terms, subreddit filters, individual creation/editing/removal, and CSV import with a validation preview. Download the template from Keywords. CSV requires `keyword,category`; optional headers are `included_terms,excluded_terms,subreddits`. Lists use semicolons, category names match the five UI categories, duplicates are scoped to the project, and invalid rows are skipped only after review. Limit: 1 MB / 1,000 rows. Existing profile keywords seed the local keyword library on first use; subsequent library edits are separate from the project's original freeform profile keywords.

Settings has four source cards with feature availability, status, last successful sync, errors and coverage limitations. The interfaces live in `lib/providers/index.ts`: approved Reddit discovery, Google search results, manual URL entry, and demo records. Manual and demo adapters work locally. Approved Reddit and Google contracts have no live implementations. No scraping, remote fetches, credential entry/storage, or assumed commercial permissions. Future adapters must run on the server with server-managed secrets and verified permissions. This static app does not provision a secret store.

Manual records are in Settings → Source records; they are explicitly user-supplied/unverified and never receive fabricated content or ranks. Demo records are fictional and their ranks simulated. All configuration and records are browser-local and project-scoped for filtering, with no authentication or server security guarantees. Archived project records are retained and hidden until restoration. Available modes have no sync timestamps because no remote sync occurs.

Verification: `npm test` covers CSV quoting/validation, duplicate handling, filter validation, safe Reddit URL normalization, manual provenance and the prior project validation tests. `npm run build` verifies TypeScript and static export. Manual acceptance: choose a project in Keywords, add/edit/delete a keyword; import the template and inspect duplicate/invalid row errors; refresh; open Settings, inspect all four provider details; add an HTTPS Reddit thread URL, switch Manual/Demo views, refresh, and confirm manual rank stays “Not checked.” Browser end-to-end QA is unverified.
