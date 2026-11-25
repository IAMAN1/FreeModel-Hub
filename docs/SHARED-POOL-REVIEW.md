# Shared Pool provider review — 25 Nov 2026

## Decision
Do not enable a multi-provider pool yet. DeepSeek Open Platform is the only adapter conditionally approved for the first beta. It is a **metered paid API, not a guaranteed free API**. Keep `SHARED_POOL_ENABLED=false` until the operator funds the account, configures a hard spend cap, confirms regional/legal eligibility, and accepts the budget.

| Provider | Terms findings for student audience | Decision |
|---|---|---|
| DeepSeek Open Platform | Official API terms explicitly permit integration into downstream applications and service to external end users. API keys must remain secret and not appear in browser code. Terms tell minors under 18 to read with a legal guardian. API is metered. | Conditional: backend-only key, guardian acknowledgement, quotas, budget cap. |
| Gemini API | Additional terms require API users to be 18+ and prohibit API clients directed to or likely accessed by under-18s. | Excluded. |
| GroqCloud | Services Agreement says cloud services are not for consumer use and requires age 18+. Resale/sublicensing needs express approval. | Excluded pending written approval. |
| OpenAI API | Agreement allows Customer Applications for End Users, prohibits API key sharing/transfer, and requires parental/guardian consent for minors. Exact shared-pool contract/account model not confirmed. | Not enabled pending legal/account review. |
| Anthropic API | Public legal center links commercial and service-specific terms; this review did not confirm permission for this shared-pool arrangement. | Not enabled pending written clarification. |
| OpenRouter | Model/upstream terms and restrictions need separate review before pooling. | Not enabled. |

## Official sources
- DeepSeek Open Platform Terms: https://cdn.deepseek.com/policies/en-US/deepseek-open-platform-terms-of-service.html
- DeepSeek API pricing: https://api-docs.deepseek.com/quick_start/pricing/
- Gemini API Additional Terms: https://ai.google.dev/gemini-api/terms
- Groq Services Agreement: https://console.groq.com/docs/legal/services-agreement
- OpenAI Services Agreement: https://openai.com/policies/services-agreement/
- Anthropic Legal Center: https://www.anthropic.com/legal

## Launch checklist
- [ ] Verify account, region, downstream use and local-law requirements with provider; obtain written approval if uncertain.
- [ ] Configure DeepSeek billing, hard spend cap and low-balance alerts.
- [ ] Configure Auth email confirmation, privacy/retention policy, deletion process, abuse controls and a valid guardian-consent process for actual age groups.
- [ ] Apply migration and deploy Edge Function with JWT verification.
- [ ] Set `DEEPSEEK_API_KEY` as server-side secret only.
- [ ] Test account isolation, RLS, quota, prompt size, error paths and deletion.
- [ ] Only then set `SHARED_POOL_ENABLED=true`.

This is an engineering screening, not legal advice or provider authorization. Re-check terms before launch.