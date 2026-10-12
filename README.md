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
