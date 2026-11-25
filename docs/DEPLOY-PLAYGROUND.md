# Deploy AI Playground

The GitHub Pages frontend is static. Auth, cloud chat and inference require a Supabase project.

1. Create/select an active Supabase project. Run `supabase/migrations/20261125000100_playground.sql` in its SQL editor.
2. Enable email/password Auth and email confirmation. Set the site URL and allowed redirects to the GitHub Pages origin (for example `https://iaman1.github.io`).
3. Set project URL and publishable/anon key in `playground-config.js`. Never use a service-role/secret key in browser code.
4. Deploy `supabase/functions/chat/index.ts` as function `chat` with JWT verification enabled.
5. Add Edge Function secrets `DEEPSEEK_API_KEY` and `SHARED_POOL_ENABLED=false`. Supabase's secret/service-role key is injected by the platform; do not commit it.
6. Open `playground.html`, register, verify email if enabled, then sign in.
7. Verify each user's age/guardian-consent evidence outside self-attestation. Only an authorized administrator may approve a profile. Example SQL for an individually reviewed account: `update public.profiles set guardian_consent=true, access_approved=true where id='<AUTH_USER_UUID>';` Never let users update these flags themselves.
8. After legal, billing, budget and security checks, enable the shared pool by changing the secret to `true`.

## Controls
- DeepSeek Flash only; provider allowlist enforced server-side.
- 15 requests/user/rolling hour using an atomic quota function.
- 12,000-character input limit; 2,048 max output tokens; 30 prior messages; 60-second timeout.
- Cloud history protected by per-user RLS.
- Provider use is metered and may incur real charges.

No Supabase project in the connected workspace is currently active and no provider key was supplied. This repository commit is deployable source, not a claim that live auth or inference is already running.