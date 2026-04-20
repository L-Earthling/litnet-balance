# Scripts

This folder contains the LLM extraction pipeline and analysis code for the thesis.

## Files

### `network_extraction.ipynb` — LLM extraction pipeline

Converts structured chapter-level narrative summaries and curated character lists
into per-book signed character-relationship networks.

**Input per book** (one subfolder per literary work):
- `<book_slug>_clean.txt` — chapter summaries, partitioned with `=== PARTITION N: [Title] ===` delimiters
- `<book_slug>_characters_clean.txt` — curated cast list (MAJOR / MINOR sections)

**Output per book**:
- `<book_slug>_network.csv` — signed edge tuples `(Chapter, Character A, Character B, Relationship)`
- `<book_slug>_processing_log.csv` — per-partition audit trail

**Key features**:
- Multi-key API rotation (round-robin across up to 6 keys)
- SQLite-backed progress tracking — crash-resilient, supports deterministic resume
- Incremental CSV writes after every partition (no data loss on interruption)
- Sequential and multi-threaded execution modes

**API keys**: set as environment variables before running — do not hardcode.

```bash
export ACADEMICCLOUD_KEY_1="your_key_here"
export DEEPINFRA_KEY_1="your_key_here"
# ... add further keys as needed
```

**Model**: Meta Llama-3.3-70B-Instruct via AcademicCloud (`chat-ai.academiccloud.de/v1`)
or DeepInfra (`api.deepinfra.com/v1/openai`).

### `analysis/network_analysis.ipynb`
Reproduces all empirical results, figures, and tables reported in the thesis. Run blocks in order: **0 → 1 → A → B → C → D → E → F → H → G**.

| Block | Content | Thesis section |
|---|---|---|
| 0 | Setup: paths, config, colour scheme | — |
| 1 | Data loading, metadata join, corpus overview | §III |
| A | Static network metrics (small-world, polarity) | §VII.1 |
| B | Temporal dynamics (density, clustering, entropy) | §VII.2–3 |
| C | Structural balance B(τ), argmin, triad census | §VII.4 |
| D | Temporal motifs, persistence, burstiness, densification | §VII.5 |
| E | Genre classification (Random Forest, feature importance, t-SNE) | §VII.6 |
| F | Historical epoch stratification, form-stratified robustness | §VII.7 |
| H | Centrality rank stability, network polarization | — |
| G | Per-book figure and summary outputs | Appendix C |

**Before running:** edit the two path variables at the top of Block 0 to point to `data/combined_networks_all.csv` and `data/combined_metadata_all.csv`.

## Setup

```bash
pip install -r requirements.txt
```

Python 3.12 recommended. Virtual environment recommended:

```bash
python3 -m venv venv
source venv/bin/activate   # on Mac/Linux
pip install -r requirements.txt
```
