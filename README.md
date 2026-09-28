# PILK automation

This public repository contains only GitHub Actions workflows. The PILK source and article archive remain in the private `lynxerinc/pilk-news` repository. A read-only deploy key stored as an Actions secret lets the runner fetch that source without publishing it here.

`probe.yml` and `private-checkout.yml` have passed. `live.yml` writes recent RSS articles directly to Cloudflare KV; `pages.yml` builds the full static site and uploads it to Cloudflare Pages. Both publishing workflows are manual-only until a scoped Cloudflare token is installed and their first runs succeed. The pages job also commits the refreshed article archive to the private repository, so a hosting move does not depend on Cloudflare KV. Nothing is committed to this public repository.

Required Actions secrets:

- `PILK_READONLY_DEPLOY_KEY` (already installed)
- `PILK_WRITE_DEPLOY_KEY` (already installed; private archive backup only)
- `CLOUDFLARE_API_TOKEN` (already installed)
- `CLOUDFLARE_ACCOUNT_ID` (already installed)
- `PILK_KV_NAMESPACE_ID` (already installed)

The Cloudflare token needs permission to edit Workers KV and Cloudflare Pages in the PILK account. No secret values belong in this repository or in workflow logs. The Cloudflare hourly refresh remains active until both publishing workflows have been verified.
