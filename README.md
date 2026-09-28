# PILK automation

This public repository contains only GitHub Actions workflows and a timestamp of the last successful daily run. The PILK source and article archive stay in the private `lynxerinc/pilk-news` repository. A read-only deploy key stored as an Actions secret lets the runner fetch that source without publishing it here.

`live.yml` fetches the current RSS feeds every 15 minutes (UTC minutes 7, 22, 37 and 52) and writes the latest articles directly to Cloudflare KV. `pages.yml` runs at 04:17 UTC each day: it restores the archive from KV, collects articles, builds the static HTML, deploys Cloudflare Pages, saves the archive to KV, then commits that archive to the private Git repository. This private commit makes the content portable to another host. The daily job also updates a harmless timestamp in this public repository to keep its scheduled workflows active.

The manual runner, private checkout, live publishing and full Pages deployment have been verified. GitHub Actions may delay scheduled runs under load, so the 15-minute cadence is a target rather than an instant push service.

Required Actions secrets, all installed:

- `PILK_READONLY_DEPLOY_KEY` (private source read access)
- `PILK_WRITE_DEPLOY_KEY` (private archive backup only)
- `CLOUDFLARE_API_TOKEN` (Workers KV and Cloudflare Pages)
- `CLOUDFLARE_ACCOUNT_ID`
- `PILK_KV_NAMESPACE_ID`

No secret values belong in this repository or in workflow logs. The old Cloudflare hourly Cron can be removed once the new schedule is observed in production.
