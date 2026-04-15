# Data

This folder contains the core datasets released with the thesis.

## Files

### `combined_networks_all.csv`
Character-relationship edge observations. One row = one character pair in one chapter.

| Column | Type | Description |
|---|---|---|
| `Book` | string | `"Author: Title"` — unique book identifier |
| `Chapter` | int | Chapter number (1-indexed) |
| `Character A` | string | First character (canonical name) |
| `Character B` | string | Second character (canonical name) |
| `Relationship` | string | `positive`, `negative`, or `neutral` |
| `source` | string | Source platform (`sparknotes` or `litcharts`) |

**Size:** 109,855 rows × 6 columns | Comma-delimited (`,`)

```python
import pandas as pd
df = pd.read_csv("combined_networks_all.csv")
```

### `combined_metadata_all.csv`
Per-work metadata. One row per literary work.

| Column | Type | Description |
|---|---|---|
| `Author` | string | Author name |
| `Book` | string | Title (matches `Book` in networks file) |
| `Year` | int | Year of first publication |
| `Epoch` | string | Historical epoch (6 categories) |
| `Genre_Primary` | string | Primary genre label (8 categories) |
| `Genre_Secondary` | string | Secondary genre label where applicable |
| `Form` | string | `Novel`, `Play`, or `Other` |
| `Remarks` | string | Curation notes |
| `source` | string | Source platform |

**Size:** 900 rows × 9 columns | Comma-delimited (`,`)

## Corpus summary

| Statistic | Value |
|---|---|
| Total works | 873 (after quality filtering) |
| Total edges | 109,855 |
| Authors | 544 |
| Genres | 8 |
| Epochs | 6 |
| Median cast size | 16 characters |
| Median chapter count | 16 |

> ⚠️ **Form–genre confound:** Tragedy/Drama (91.8% plays) and Comedy/Satire (66.7% plays) are play-dominated; all other genres are predominantly novels. Cross-genre structural comparisons involving these two genres must be interpreted accordingly. See thesis Chapter VIII for discussion.
