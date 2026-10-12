# Quant Atlas collector

This repository runs the paper catalog refresh for [Quant Research Atlas](https://quant-research-atlas-2026.snu-chatgpt-5678.chatgpt.site/research) every day at 09:17 Asia/Seoul, even when the owner's PC is off. The Site stores papers in its D1 database; this repository stores only the scheduler.

## One-time setup

Add two **repository secrets** in **Settings → Secrets and variables → Actions**:

- `QUANT_ATLAS_SITE_ACCESS_TOKEN`: the Site service access token. It grants the runner access through the Site's workspace gate.
- `QUANT_ATLAS_JOB_TOKEN`: the Site's dedicated collection token. The worker rejects requests without it.

Do not commit either value. After setting both, run **Actions → Quant Atlas daily collection → Run workflow**. A successful run should show each refreshed topic or `0 theme(s) refreshed` when all topics are current. The workflow retries temporary HTTP 429 and 5xx errors. It does not generate unverified Korean paper summaries.

A successful workflow run is only an execution check. Confirm the Site's latest collection time advances after the 20-hour freshness window. Keep the PC visit task active until that is verified. GitHub may delay scheduled jobs, and [public-repository schedules can be disabled after 60 days without repository activity](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/disable-and-enable-workflows).
