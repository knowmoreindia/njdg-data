# NJDG data

Pending cases in India's district and taluka courts, for every court district, read from the [National Judicial Data Grid](https://njdg.ecourts.gov.in/njdg_v3/). A light reading is kept every day and the full detail once a week.

Latest weekly snapshot: 2026-W46, read on 2026-11-09. 765 districts, 5,21,75,978 pending cases (1,12,59,631 civil, 4,09,16,347 criminal).

NJDG shows only today's figures and keeps no public history. This repository is a record of how they move.

## Files

| Path | Holds |
|---|---|
| `data/daily/<YYYY-MM>.csv` | One row per district per day: pending civil and criminal, 5 age bands, cases filed and disposed of in the last month (disposals split into contested and uncontested), cases listed that day, undated and excessive-dated cases, cases filed by women and by senior citizens, and pre-litigation and pre-trial matters |
| `data/daily/gaps.csv` | Districts that have no daily row on a day, with the reason |
| `data/weekly/<ISO week>/districts.csv` | One row per district: pending civil, criminal and total, and cases older than 10 years |
| `data/weekly/<ISO week>/district-measures.csv` | One row per district, case kind and figure: 7 age bands, case groups, stages, pending by filing year, case types, purpose of the next hearing, and cases filed and disposed of each year |
| `data/districts-history.csv` | Every weekly `districts.csv` in one file |
| `data/latest/` | The newest weekly files, plus the full snapshot as JSON |
| `data/runs.csv` | Every run: when, how many requests, rows and gaps, and whether it failed |
| `codebook.csv`, `datapackage.json` | Every column, its type and what it counts. The second is a [Frictionless data package](https://datapackage.org) |
| `CHANGELOG.md` | One entry per weekly snapshot |

1 weekly snapshot so far.

## How the figures are read

- **Daily:** one request per district to NJDG's District and Taluka Courts dashboard, plus one per State. The page shows civil and criminal side by side.
- **Weekly:** NJDG's pending dashboard, once for civil and once for criminal cases in each district. Pending is the sum of the age bands NJDG returns.
- **Dates:** NJDG prints no as-of date for these figures, so each reading is dated by the day it was read, in India. A week keeps its last weekly reading.
- **Districts** are court districts as NJDG names them. Some differ from revenue districts.
- **Checks:** a reading is kept only when each State's districts add up to within 0.1% of that State's own total, the age bands add up to pending, and contested plus uncontested disposals equal disposals. A reading that fails is not written, and `runs.csv` records why.
- **Gaps:** a district that could not be read is listed in `gaps.csv`, never filled with zero. Where NJDG shows no figure the cell is empty.
- **Daily and weekly age bands differ:** the daily page has 5 bands (everything over 10 years together), the weekly detail has 7. On the daily page the bands can add up to slightly more than the pending figure NJDG prints beside them (by at most 0.22% on 9 November 2026). Both are kept as NJDG shows them. The daily pending figure matches the weekly one.
- **Case types** are NJDG's main types, so they can add up to slightly less than pending. **Stages** and **purposes** leave out cases with none recorded. A case type or purpose label can appear twice when NJDG lists it twice.
- `filed` and `disposed` count the current year only up to the day read.
- NJDG's delay-reason figures are the same on every district's page, so they are national and not included. Nothing behind NJDG's CAPTCHA is read, and there is no case-level or party data.

## Credit and licence

Source: National Judicial Data Grid (njdg.ecourts.gov.in), eCourts Services. This compilation is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Credit "Know More India, from National Judicial Data Grid data".
