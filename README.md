# Quant Atlas collector

This repository runs the paper catalog refresh for [Quant Research Atlas](https://quant-research-atlas-2026.snu-chatgpt-5678.chatgpt.site/research) every day at **09:17 Asia/Seoul**, even when the owner's PC is off. The Site stores papers in its D1 database; this repository stores only the scheduler.

## Current state

- The workflow and two encrypted GitHub Actions secrets were registered on 2026-10-13 KST.
- [The first manual run](https://github.com/do0405/quant-atlas/actions/runs/38279187791) completed successfully. It refreshed zero topics because the catalog was still inside its 20-hour freshness window.
- The previous Windows automatic visit task was disabled, so it cannot hide a failed GitHub schedule.
- A scheduled run that writes a new D1 collection record has **not yet been observed**. Check the Actions run history after the next eligible collection window and the Site's latest collection time.

## Operation

The workflow calls the Site's authenticated collector, which retries temporary HTTP 429 and 5xx errors. Each call updates one stale topic; one run checks up to ten topics. It does not generate unverified Korean paper summaries.

To test, open **Actions → Quant Atlas daily collection → Run workflow**. A successful run shows each refreshed topic or `0 theme(s) refreshed` when all topics are current. If a scheduled run fails, check its logs and the Site's collection status. GitHub may delay scheduled jobs; [public-repository schedules can be disabled after 60 days without repository activity](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/disable-and-enable-workflows).

## Secrets

The workflow requires two repository secrets: `QUANT_ATLAS_SITE_ACCESS_TOKEN` and `QUANT_ATLAS_JOB_TOKEN`. They are already configured. Never commit either value. If one is rotated in the Site, update the matching secret in **Settings → Secrets and variables → Actions**.
