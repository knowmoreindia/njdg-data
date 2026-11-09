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
| `data/daily/gaps.csv` | Districts missing on a given day, and why |
| `data/runs.csv` | A log of every read, including ones that failed |
| `codebook.csv` | Every column and what it counts (also as `datapackage.json`) |

1 weekly snapshot so far. Daily readings start on 9 November 2026.

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
