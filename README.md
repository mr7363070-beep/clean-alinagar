# ক্লিন আলীনগর (Clean Alinagar)

React + Vite + TypeScript + Tailwind, Supabase (Auth, Postgres, Storage), Netlify.

> **Status:** first scaffold. Not yet installed, built, or tested (no network in the authoring environment). Follow the checklist below before relying on it.

## Setup
1. `npm install`, then copy `.env.example` to `.env` and fill in your Supabase URL and **anon** key.
2. In Supabase → SQL Editor, run `supabase/migrations/0001_schema_rls.sql`.
3. `npm run dev`.

## First admin (no default password exists)
1. Supabase → Authentication → Users → **Add user** (email + strong password; or invite).
2. Disable public sign-ups: Authentication → Providers/Settings → turn off "Allow new users to sign up".
3. In SQL Editor run (replace the email):
   `insert into public.user_roles (user_id, role) select id, 'admin' from auth.users where email = 'you@example.com';`
4. Sign in at `/admin/login`.

## Netlify
Connect the GitHub repo; build `npm run build`, publish `dist` (already in `netlify.toml`, which also provides the SPA fallback so `/admin` refreshes work). Add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in Site settings → Environment variables. In Supabase → Authentication → URL Configuration, set Site URL to your Netlify/custom domain and add `https://YOUR-DOMAIN/admin/login` to Redirect URLs. Custom domain: Netlify → Domain management.

## Testing checklist
- [ ] `npm run build` succeeds
- [ ] Anonymous: can read only `status='published'` rows; cannot read `volunteers`; cannot insert posts
- [ ] Anonymous can submit the volunteer form; row appears for admin only
- [ ] Non-admin signed-in user cannot write any table or upload to `public-media`
- [ ] Admin: create/publish/unpublish/delete post; counts update; audit_logs rows written
- [ ] Refresh `/admin` and `/notices` on Netlify: no 404
