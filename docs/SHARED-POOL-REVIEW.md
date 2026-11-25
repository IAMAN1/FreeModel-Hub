# Shared Pool provider review — 25 Nov 2026

## Current decision

**Do not enable Shared Pool yet.** Supabase project and the authenticated Edge Function are deployed, but OrcaRouter key must be rotated and entered as a server-side secret. More importantly, the provider's age clause and upstream provider policies need written clarification for a student-facing service.

## OrcaRouter review

- Official API base URL: `https://api.orcarouter.ai/v1` (OpenAI-compatible Chat Completions).
- Candidate model: `orcarouter/free`, described in OrcaRouter's model catalogue as a router across its free models with rate-limited usage. Free availability is not a guaranteed SLA.
- The OrcaRouter Terms of Service (effective 18 April 2026) state that an account user must be at least 16 **or the age of majority in their jurisdiction, whichever is higher**. Thai Civil and Commercial Code Section 19 sets majority at 20. Do not assume guardian consent overrides this account eligibility clause.
- OrcaRouter Terms also say upstream provider policies apply to routed requests and that the account holder is responsible for those policies. A generic router can choose different upstream models; the exact age restrictions and permitted public downstream use therefore need assessment.
- The Terms prohibit sharing API keys through insecure channels. The key posted in the ChatGPT conversation must be revoked/rotated. Only the replacement key belongs in Supabase Edge Function Secrets as `ORCAROUTER_API_KEY`.
- OrcaRouter's Terms describe the platform as a gateway routing to upstream providers at their published prices; its public catalogue labels `orcarouter/free` as a free router, but rate limits and route availability can change. Do not promise unlimited free inference.

### Decision
**Conditional / disabled** for FreeModel Hub's student audience until OrcaRouter confirms in writing:
1. The account holder's eligibility where the operator is based in Thailand.
2. Whether a public downstream app may send prompts from student end users through one server-side API key (shared pool).
3. Which upstream models `orcarouter/free` can route to, and whether those routes are suitable for the intended age range.
4. Whether the provider requires additional notices, consent, or safeguards for end users who are minors.

## Other provider screening

| Provider | Decision |
|---|---|
| Gemini API | Excluded from a student-facing direct adapter pending age policy review; its Additional Terms have a strict 18+ restriction for API users and restrictions on clients directed to minors. |
| GroqCloud | Excluded pending written approval; its Services Agreement contains age/consumer-use limitations. |
| OpenAI API | Not enabled pending review of the exact shared-pool customer application and minor-user model. |
| Anthropic API | Not enabled pending review of upstream policy and downstream access model. |
| OpenRouter | Not enabled; do not confuse OpenRouter with OrcaRouter. They are different services. |

## Official sources
- OrcaRouter Terms: https://www.orcarouter.ai/terms.html
- OrcaRouter API / product: https://www.orcarouter.ai/
- OrcaRouter models and free router: https://www.orcarouter.ai/models
- OrcaRouter support: https://www.orcarouter.ai/support
- Thai Civil and Commercial Code, Section 19: https://library.siam-legal.com/thai-law/civil-and-commercial-code-natural-persons-sections-15-36/
- Gemini API Additional Terms: https://ai.google.dev/gemini-api/terms
- Groq Services Agreement: https://console.groq.com/docs/legal/services-agreement
- OpenAI Services Agreement: https://openai.com/policies/services-agreement/
- Anthropic Legal Center: https://www.anthropic.com/legal

## Launch checklist
- [ ] Revoke the API key posted in chat and create a new OrcaRouter key.
- [ ] Obtain written confirmation from OrcaRouter for the public downstream shared-pool/student use case and account eligibility.
- [ ] Review each upstream route/model's age and acceptable-use terms; replace `orcarouter/free` with a fixed allowlist if necessary.
- [ ] Set the replacement key as Supabase Edge Function secret `ORCAROUTER_API_KEY`; never commit it.
- [ ] Configure a hard workspace spend cap and usage alerts. If using only the free router, verify the route is still zero-cost at runtime.
- [ ] Configure Auth email confirmation, privacy/retention policy, account deletion and moderation/abuse reporting.
- [ ] Implement age eligibility and a real guardian-consent workflow if required; a checkbox alone is not age verification or parental consent.
- [ ] Test tenant isolation, RLS, quota, request limits, timeouts and deletion.
- [ ] Only after all above checks, set `SHARED_POOL_ENABLED=true`.

This is an engineering screening, not legal advice or provider authorization.
