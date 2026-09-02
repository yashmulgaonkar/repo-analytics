# repo-analytics

Copyright (c) 2026 Yash Mulgaonkar <yashmulgaonkar@gmail.com>

All-time GitHub repository analytics for **FlightScnr**, **FlightScnr_Pi**, and **halo**.
Collects clones/views beyond GitHub’s 14-day traffic window and renders a
Repobeats-style SVG panel for each README.

**License:** [CC BY-NC-SA 4.0](LICENSE) — see also [NOTICE](NOTICE).
Commercial use is not permitted without separate written permission.

## Embed

After the hourly workflow has run at least once:

```markdown
![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr/analytics.svg)
![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr_Pi/analytics.svg)
![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/halo/analytics.svg)
```

## How it works

1. GitHub Actions runs hourly (`17 * * * *`).
2. `scripts/collect.py` pulls traffic + issues/PRs/commits and upserts daily traffic into `data/`.
3. `scripts/render.py` writes `out/<repo>/analytics.svg`.
4. Changes are committed back to this repo.

Clone/view **all-time** totals start from the first successful sync (plus whatever remains in GitHub’s current 14-day window). Issues, PRs, and commits are rebuilt from the API each run.

## Setup

1. Create a classic PAT with the `repo` scope (required for the Traffic API).
2. Add it as repository secret `TRAFFIC_TOKEN`.
3. Run **Actions → Update analytics → Run workflow**.

## Add another repo

Edit [`repos.yaml`](repos.yaml), then re-run the workflow.

## Local run

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export TRAFFIC_TOKEN=ghp_...
python3 scripts/collect.py
python3 scripts/render.py
```

## Author

Yash Mulgaonkar — yashmulgaonkar@gmail.com  
https://github.com/yashmulgaonkar/repo-analytics
