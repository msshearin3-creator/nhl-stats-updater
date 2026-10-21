# NHL Stats Updater

A scheduled GitHub Action pulls current NHL team stats twice a day and saves them in `data/`. Your Excel model reads the CSV straight from GitHub, so updating is just Refresh All. Nothing runs on your computer.

## What runs and when

| When (ET) | Why |
|---|---|
| About 2:15am | After the late West Coast games finish |
| About 7:45am | After the NHL stats feed catches up overnight |

GitHub can start scheduled runs 10 to 30 minutes late at busy times. To run it right now: **Actions** tab, **Update NHL stats**, **Run workflow**.

## Files it writes

| File | What it is |
|---|---|
| `data/nhl_teams.csv` | Latest snapshot, one row per team. This is what Excel reads. |
| `data/nhl_teams.xlsx` | Same data as an Excel table named `NHL_Teams`. |
| `data/history/YYYY-MM-DD.csv` | One snapshot per day. Use these to backtest with the stats as they stood on a given date. |

Columns: Team, Abbrev, Conf, Div, GP, W, L, OTL, PTS, P%, RW, ROW, GF, GA, GD, GF/GP, GA/GP, SF/GP, SA/GP, PP%, PK%, FO%, CF%, FF%, CF% Close, 5v5 Sh%, 5v5 Sv%, PDO, OZ Start%, home and road splits, last 10, Streak, Updated.

Percent columns are decimals (0.536 = 53.6%).

## Connect Excel

1. In Excel: **Data**, **From Web**.
2. Paste this URL:
   `https://raw.githubusercontent.com/msshearin3-creator/nhl-stats-updater/main/data/nhl_teams.csv`
3. Click **Load**. Excel creates a table you can point your model at.
4. Optional: **Data**, **Queries & Connections**, right-click the query, **Properties**, tick **Refresh data when opening the file**.

The repo needs to be **public** for this URL to work without a login. The data is all public NHL stats.

## Sources

NHL public feeds, no API key: `api-web.nhle.com/v1/standings/now`, `api.nhle.com/stats/rest/en/team/summary`, `api.nhle.com/stats/rest/en/team/percentages`. They are unofficial, so if the NHL changes them the Action will fail and GitHub will email you.

## Run it yourself

```
pip install -r requirements.txt
python scripts/update_nhl.py
```
