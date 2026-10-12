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

## Phase 4 — Opportunities UI
The dedicated Opportunities page combines fictional demo discussions and locally entered manual URLs, with searchable table/card views. All requested context fields are displayed, including explicitly unknown URLs/metrics/excerpts/publication dates for records without evidence. Demo URLs are unavailable rather than fabricated links to actual threads. Demo publication/discovery/check times and classifications are sample data; their freshness uses a fixed Oct 12, 2026 snapshot, not an invented live sync.

Filters: client, matched keyword, subreddit, intent, publication or discovery date range, assignee, workflow status and data source. Date filtering uses America/Los_Angeles and excludes unknown dates when a range is set. Workflow: New, Reviewing, Drafting, Approved, Responded, Dismissed. Ownership options are explicitly demo team members. Browser-local overrides retain workflow, assignees, notes, and manual drafts. Demo response actions open the existing drafting workflow. Manual drafts are saved in their opportunity detail panels.

Opportunity scoring exposes four weights: business relevance 40%, buying intent 25%, freshness 20%, and available Google visibility 15%. Unknown factors stay null and do not contribute; remaining weights are normalized with evidence coverage displayed. With no evidence the score is Unknown. Demo business relevance/intent/rank inputs are illustrative. Manual records have no fabricated scoring inputs. Deduplication is per project using stable source IDs or Reddit thread IDs extracted from canonical URL variants, preserving a discussion separately for different clients.

Verification: `npm test` now includes score coverage/unknown behavior, deterministic demo scoring and ID/canonical URL deduplication. Build includes TypeScript verification. Manual acceptance: add a manual URL in Settings, inspect it in Opportunities, compare table/cards, filter each field, inspect score factors, assign a demo teammate, change status, add notes, save a draft, refresh and confirm local activity. Check a manual record has unknown comments/upvotes/rank/publication/check times. Live discovery, scoring classification, rankings, team identity/sync and posting remain disconnected. Browser end-to-end QA remains unverified.
