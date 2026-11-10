# NJDG data

How many cases are pending in each of India's 765 court districts, read from the [National Judicial Data Grid](https://njdg.ecourts.gov.in/njdg_v3/) every day, with a fuller breakdown every week.

NJDG only ever shows today's numbers. This repo keeps the days.

**Latest week (2026-W46, read 2026-11-09):** 5.22 crore cases pending in district and taluka courts, 5,21,75,978 in all. 1.13 crore civil, 4.09 crore criminal.

## Quick start

```python
import pandas as pd

base = "https://raw.githubusercontent.com/knowmoreindia/njdg-data/main/data"
daily = pd.read_csv(f"{base}/daily/2026-11.csv")
weekly = pd.read_csv(f"{base}/latest/districts.csv")
```

Each district has a stable `id` like `bihar/patna`, the same in every file.

## Files

| Path | What's in it |
|---|---|
| `data/daily/<YYYY-MM>.csv` | A row per district per day: pending civil and criminal cases, how old they are (5 bands), cases filed and disposed of last month, cases listed that day, and cases filed by women and senior citizens |
| `data/weekly/<week>/districts.csv` | A row per district: pending civil, criminal and total, and cases over 10 years old |
| `data/weekly/<week>/district-measures.csv` | The weekly detail in long form: 7 age bands, case types, stages, year of filing, purpose of the next hearing, and cases filed and disposed of each year |
| `data/districts-history.csv` | Every weekly `districts.csv` stacked |
| `data/latest/` | The newest weekly files, and the full snapshot as JSON |
| `data/national/history.csv` | All-India figures back to 22 November 2018, 293 days so far. Before 9 November 2026 each row comes from a copy of NJDG's page saved on the Internet Archive, linked in `source` |
| `data/daily/gaps.csv` | Districts missing on a given day, and why |
| `data/runs.csv` | A log of every read, including ones that failed |
| `codebook.csv` | Every column and what it counts (also as `datapackage.json`) |

1 weekly snapshot so far. District readings start on 9 November 2026.

## Going further back

District figures start when this repo did, because NJDG doesn't publish its past. For India as a whole, the Internet Archive saved NJDG's dashboard on 290 days between 22 November 2018 and 9 November 2026, and `data/national/history.csv` holds what those copies show.

- They're snapshots on scattered days, not a daily series. Some months have none.
- Until 2 September 2024 NJDG had an older dashboard (`page` is `old_dashboard`). It showed age bands (4 until late 2018, then 7), case types and stages, but not hearings, contested disposals or pre-litigation. Columns a page didn't show are blank, and its civil and criminal figures can miss its own printed total by up to half a percent.
- No copy survives from August 2023 to April 2024. One day in that stretch, 30 November 2023, comes from a Rajya Sabha answer quoting NJDG (`page` is `parliament_answer`).
- Copies that couldn't be read are listed in `data/national/gaps.csv`, along with two days when NJDG showed only about half its usual total.
- Where they overlap, these agree to within 0.02% with the 2021 readings in [apoorv74/NJDG-Stats](https://github.com/apoorv74/NJDG-Stats).

## Worth knowing

- Readings are dated by the day they were taken, in India time. NJDG doesn't say when its figures were last updated.
- These are court districts as NJDG lists them, which don't always match revenue districts.
- A district that couldn't be read is left out and listed in `gaps.csv`. Nothing is filled in or estimated. A blank cell means NJDG showed no figure.
- The daily page shows 5 age bands, the weekly detail 7. On the daily page NJDG's bands can add up to slightly more than the pending figure beside them (at most 0.22% when this started). Both are kept as shown.
- Case types cover NJDG's main types only, so they can add up to a little less than pending. Stages and purposes skip cases with none recorded, and NJDG sometimes lists the same label twice.
- `filed` and `disposed` for the current year run up to the day read.
- No case-level or party data, and nothing behind NJDG's CAPTCHA.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit "Know More India, from National Judicial Data Grid data". The underlying figures are published by eCourts Services on njdg.ecourts.gov.in.
