# PILK automation

This public repository contains only GitHub Actions workflows. The source code and the complete article archive live in the private `lynxerinc/pilk-news` repository. Cloudflare KV is a delivery cache and secondary backup, not the source of truth.

A small Cloudflare Worker starts `live.yml` at UTC minutes 7, 22, 37 and 52. The job checks out the private repository, fetches RSS, commits and pushes `data/archive.json` there only when articles changed, and then publishes the prepared latest articles to Cloudflare KV. A manually added article in the private archive is preserved and can appear in the live feed on the next pass.

The same Worker starts `pages.yml` every four hours at UTC minute 17. That job checks out the current private repository, merges new RSS articles, commits any archive change before publication, updates the live KV data, builds the static HTML, deploys Cloudflare Pages and stores another compressed archive copy in KV. At six Pages deployments per day, the cadence is about 180 per month, below the Free plan's 500-deployment limit.

Both workflows can also be started manually from the Actions tab. They use the same concurrency group to avoid concurrent archive commits. If the private repository changes during a collection, the job fails safely and retries on the next scheduled run instead of overwriting that change.

Required Actions secrets, all installed:

- `PILK_READONLY_DEPLOY_KEY` (private source read access)
- `PILK_WRITE_DEPLOY_KEY` (private archive write access)
- `CLOUDFLARE_API_TOKEN` (Workers KV and Cloudflare Pages)
- `CLOUDFLARE_ACCOUNT_ID`
- `PILK_KV_NAMESPACE_ID`

No secret values belong in this repository or in workflow logs.
