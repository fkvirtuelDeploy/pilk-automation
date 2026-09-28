# PILK automation

This public repository contains only GitHub Actions workflows. The PILK source and article archive stay in the private `lynxerinc/pilk-news` repository. The workflows fetch the private source with a read-only deploy key; no source or articles are committed here.

A small Cloudflare Worker triggers `live.yml` at UTC minutes 7, 22, 37 and 52. That job collects the current RSS feeds and writes recent articles directly to Cloudflare KV. The same Worker triggers `pages.yml` daily at 04:17 UTC. The daily job restores the archive, collects articles, builds and deploys the full static site, saves the archive to KV, and commits `data/archive.json` to the private repository. That Git backup keeps the content portable to another host.

Both workflows can also be started manually from the Actions tab. The live workflow was verified end to end on scheduled Cloudflare triggers at 15:37 and 15:52 UTC on September 28, 2026. These times are a target cadence; RSS publication and job execution can introduce delay.

Required Actions secrets, all installed:

- `PILK_READONLY_DEPLOY_KEY` (private source read access)
- `PILK_WRITE_DEPLOY_KEY` (private archive backup only)
- `CLOUDFLARE_API_TOKEN` (Workers KV and Cloudflare Pages)
- `CLOUDFLARE_ACCOUNT_ID`
- `PILK_KV_NAMESPACE_ID`

No secret values belong in this repository or in workflow logs.
