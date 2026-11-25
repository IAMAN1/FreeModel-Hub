# Deploy AI Playground

Supabase project `freemodel-hub` is provisioned in `ap-south-1`. The schema and authenticated `chat` Edge Function have been deployed. Shared Pool remains disabled.

## Frontend
1. The public Supabase URL and publishable key are configured in `playground-config.js`.
2. The frontend is on Git branch `feature/ai-playground`; merge the reviewed PR into `main` before it appears on GitHub Pages.
3. In Supabase Auth settings, enable email/password, require email confirmation, set Site URL to the deployed GitHub Pages origin, and add the exact Playground URL to allowed redirects.

## Provider secret
1. Revoke the API key that was pasted into a chat and create a replacement in OrcaRouter.
2. In Supabase Dashboard → Project `cowvhtclmgduchkwaijr` → Edge Functions → Secrets, add:
   - `ORCAROUTER_API_KEY` = replacement key
   - `SHARED_POOL_ENABLED` = `false`
3. Never put the provider key in GitHub, `playground-config.js`, browser storage, or client-side code.
4. The current adapter uses `https://api.orcarouter.ai/v1/chat/completions` and the `orcarouter/free` model route.

## Student audience / terms gate
Do not change `SHARED_POOL_ENABLED` to `true` until OrcaRouter has confirmed in writing that this downstream shared-pool design is permitted and the age eligibility is compatible with the intended student audience. OrcaRouter Terms say at least 16 or the local age of majority, whichever is higher; Thai majority is 20. Upstream provider policies also apply to routed calls. A guardian checkbox is not a substitute for eligibility verification or a legally valid consent process.

## Current safeguards
- Supabase Auth JWT required by Edge Function gateway.
- Per-user RLS for conversations and messages.
- Server-side profile approval + guardian-consent gate.
- Server-side-only OrcaRouter key.
- `orcarouter/free` model allowlist; no user-supplied arbitrary model.
- 15 requests/user/rolling hour, atomic quota function.
- 12,000-character prompt cap, 2,048 output tokens, 30 prior messages, 60-second timeout.
- Usage events inaccessible to browser roles.

## Current status
- Supabase project: ACTIVE_HEALTHY.
- Migration: applied and schema verified.
- Edge Function `chat`: ACTIVE, JWT verification enabled.
- Security advisor: no findings after RLS policy correction.
- Performance advisor: one non-critical recommendation to add an index on `messages.user_id`; remaining unused-index notices are expected on a new empty database.
- Provider secret: not configured. Shared Pool: disabled.
- Live inference test: pending a rotated key and written provider/age-policy clearance.
