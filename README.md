# PILK automation

Public scheduling repository for PILK. The site source remains in the private `lynxerinc/pilk-news` repository. This repository contains only automation workflows, never the private source or generated news.

The manual runner probe verifies that standard GitHub-hosted runners start on this account. The private checkout probe uses a read-only deploy key stored as a GitHub Actions secret. Neither probe publishes to Cloudflare.
