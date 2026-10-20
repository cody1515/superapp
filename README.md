# SitePilot Cloud Scanner

## What this changes
- The existing superintendent dashboard and local project data remain.
- The Scan Schedule tab now sends up to three selected schedule images at a time to a Netlify serverless function, which uses a vision AI model to extract a Microsoft Project table.
- Each row is reviewed with editable name, dates, duration, and predecessor IDs before import. Low-confidence fields are unchecked by default.
- AI API keys are NEVER included in the browser app.

## Deploy (important)
**Netlify Drop / drag-and-drop ZIP deployment does not reliably deploy Netlify Functions.** Use a Git-connected Netlify deployment (GitHub repository) or Netlify CLI.

1. Make a **private GitHub repository** and upload the files and `netlify/functions` directory from this ZIP, keeping the folder structure. Do not upload your real schedule photos or any API keys.
2. On Netlify, Add new project > Import an existing project > GitHub, and select that repository. Publish directory is `.`; Netlify reads `netlify.toml` and deploys the function.
3. Netlify > Project configuration > Environment variables: add `OPENAI_API_KEY` (from your own OpenAI API account) and `SITEPILOT_ACCESS_CODE` (a long, unique private passphrase). Set scope to Functions or All. Do NOT paste either into HTML or GitHub.
4. Trigger a fresh deployment after adding variables. Visit `https://YOUR-SITE.netlify.app` on iPhone Safari.
5. In Scan Schedule, enter your access code, select 1–3 photos, and tap **Scan with AI**. Review all rows and dates before importing.

## Security / limitations
- Shared access code is a **basic gate**, not user accounts or enterprise authentication. For a production system, use proper authentication, abuse controls, quotas, and audit logs. Anyone with the code can use your AI scanning allowance.
- Your photos are sent over HTTPS to Netlify's function and to the AI provider for processing. Obtain company permission before uploading job documents.
- No photos are stored by this function; normal provider/platform operational logging and retention policies may apply.
- Project data is still saved in the iPhone's browser localStorage. Export a backup before upgrading. Cloud data sync and push notifications are NOT included.
- AI output can be wrong; confirm task names, dates, inspections, and dependencies against the original printed schedule. If some pages are tiny, move closer and scan each page separately.
- Your OpenAI API account must have billing enabled; model availability and pricing vary. `SITEPILOT_VISION_MODEL` optionally overrides `gpt-4.1`.
- The function limits image count and size but does not implement per-user rate limits or hard spend caps. Set a spending limit/alert in your API account.
