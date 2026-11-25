# FreeModel Hub

Thai-first directory for discovering AI models and providers with free hosted API access, limited trials, or open weights.

## Run locally
No build step is required. Open `index.html` in a browser, or run a static server:

```bash
python -m http.server 8000
```

Then visit http://localhost:8000.

## Deploy with GitHub Pages
1. Open **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select `main` and `/ (root)`, then Save.
4. Wait for the Pages deployment to complete.

## Features
- Search and filter by provider, task, and access type
- Provider directory with official documentation links
- Model detail pages with caveats and API template
- Compare up to three models
- Favorites stored locally in the current browser
- Responsive layout and dark mode

## Important data policy
- "Free" means a provider currently documents a free tier, free model route, developer allowance, or evaluation access. It never means unlimited.
- Free access is classified separately from open weights. Running open-weight models yourself still requires compute.
- Limits, model availability, regional eligibility, privacy terms, and API IDs can change. Always verify the official pricing, rate-limit, and model documentation linked on each entry.
- This is a curated directory snapshot dated **2026-11-25**, not a live quota monitor. Entries that depend on account eligibility or trial access are labeled accordingly.
- Model names such as "Gemini Flash" and "Qwen (Groq hosted)" are family-level directory entries. Use the exact currently supported model ID shown by the provider console/docs.
- Do not use free-tier services for sensitive or regulated data without reviewing the provider's data handling terms.

## Security
The site does not collect or store API keys. The quickstart is a placeholder template. Never commit secrets or expose provider keys in browser-side JavaScript.

## Sources
Provider links point to official docs/pricing pages for Google AI, Groq, OpenRouter, Cloudflare, NVIDIA, Z.ai, Cohere, and SambaNova. Re-check source pages before relying on a particular quota or model ID.

## License
MIT