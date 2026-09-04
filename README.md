# repo-analytics

Copyright (c) 2026 Yash Mulgaonkar <yashmulgaonkar@gmail.com>

All-time GitHub repository analytics: collect clones/views beyond GitHub’s
14-day traffic window and render a Repobeats-style SVG panel for each README.

This repository is a **template**. Use it for your own repos, or follow the
live instance below for [FlightScnr](https://github.com/yashmulgaonkar/FlightScnr),
[FlightScnr_Pi](https://github.com/yashmulgaonkar/FlightScnr_Pi), and
[halo](https://github.com/yashmulgaonkar/halo).

**License:** [CC BY-NC-SA 4.0](LICENSE) — see also [NOTICE](NOTICE).
Commercial use is not permitted without separate written permission.
When you fork or adapt this work, keep attribution and ShareAlike terms.

## Use as a template

1. Click **Use this template** (or fork) to create your own copy.
2. Edit [`repos.yaml`](repos.yaml) (see [`repos.example.yaml`](repos.example.yaml)) with `owner/name` entries you can access.
3. Create a **classic PAT** with the `repo` scope (required for the Traffic API).
4. Add it as Actions secret `TRAFFIC_TOKEN` (Settings → Secrets and variables → Actions).
5. Optional Actions **variables**:
   - `GIT_AUTHOR_NAME` / `GIT_AUTHOR_EMAIL` — commit identity for analytics refreshes (defaults to the user who triggered the workflow / `github.actor`).
6. Optional cleanup after templating: delete contents of `data/` and `out/` (keep the folders) so your first run starts clean.
7. Run **Actions → Update analytics → Run workflow**.
8. Embed each SVG in the target README (replace `YOUR_USER` / `YOUR_ANALYTICS_REPO` / `RepoName`).
   Wrap the image so **clicking the panel opens the SVG** (same URL for src and href):

```markdown
[![Repo analytics](https://raw.githubusercontent.com/YOUR_USER/YOUR_ANALYTICS_REPO/main/out/RepoName/analytics.svg)](https://raw.githubusercontent.com/YOUR_USER/YOUR_ANALYTICS_REPO/main/out/RepoName/analytics.svg)
```

Each panel includes a footer CTA (“Make an analytics dashboard for your repo”) linking to this
template (`REPO_ANALYTICS_URL`). That SVG-internal link works when the raw SVG is open;
GitHub README `<img>` embeds do not activate SVG-internal links, so the README wrap
above opens the SVG, and the CTA is usable from there.

To change how often it runs, edit the `cron:` in
[`.github/workflows/update-analytics.yml`](.github/workflows/update-analytics.yml)
(GitHub Actions does not expand repository variables inside schedule triggers).

## Live embeds (this instance)

After the hourly workflow has run at least once:

```markdown
[![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr/analytics.svg)](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr/analytics.svg)
[![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr_Pi/analytics.svg)](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/FlightScnr_Pi/analytics.svg)
[![Repo analytics](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/halo/analytics.svg)](https://raw.githubusercontent.com/yashmulgaonkar/repo-analytics/main/out/halo/analytics.svg)
```

## How it works

1. GitHub Actions runs on a schedule (default hourly at `17 * * * *`) or via workflow_dispatch.
2. `scripts/collect.py` pulls traffic + issues/PRs/commits and upserts daily traffic into `data/`.
3. `scripts/render.py` writes `out/<repo>/analytics.svg`.
4. Changes are committed back to this repo.

Clone/view **all-time** totals start from the first successful sync (plus whatever remains in GitHub’s current 14-day window). Issues, PRs, and commits are rebuilt from the API each run.

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
