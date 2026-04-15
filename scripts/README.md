# Scripts

This folder contains the extraction pipeline and analysis code.

## Files

### `6_create_text_structure.py`
Structures raw LitCharts text content into the standardized partition format required by the extraction pipeline. Each book's content is split into chapter partitions delimited by `=== PARTITION N: [Title] ===`.

**Usage:**
```bash
python 6_create_text_structure.py --input_dir /path/to/raw_text --output_dir /path/to/structured
```

### `7_extract_networks.py`
LLM-based extraction pipeline. Processes each chapter partition using Meta Llama-3.3-70B-Instruct to extract signed character-relationship tuples (A, B, σ, t).

**Features:**
- Multi-key API rotation (round-robin across 6 keys)
- SQLite progress tracking (crash-resilient, supports resume)
- Incremental CSV writes (no data loss on interruption)
- Configurable rate limiting (13 requests/minute per key)

**Requirements:**
- AcademicCloud API key (`chat-ai.academiccloud.de/v1`) or DeepInfra API key (`api.deepinfra.com/v1/openai`)
- Set your key as an environment variable: `export OPENAI_API_KEY=your_key_here`

**Usage:**
```bash
python 7_extract_networks.py --books_dir /path/to/structured_text --output_dir /path/to/networks
```

### `analysis/CharDyNet_Analysis.ipynb`
Full analysis notebook. Run blocks in order:

| Block | Content |
|---|---|
| Block 0 | Setup: directories, colour scheme, logging |
| Block 1 | Corpus loading and static network metrics |
| Block A | Aggregate static analysis (Table VII.1, Figure VII.1) |
| Block B | Temporal network metrics (density, polarity, clustering, entropy) |
| Block C | Structural balance B(τ): trajectories, argmin, triad census |
| Block D | Temporal motifs and densification |
| Block E | Genre classification (Random Forest, baselines, feature importance) |
| Block F | Epoch stratification and form-stratified robustness checks |
| Block G | Per-book case study outputs |

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
