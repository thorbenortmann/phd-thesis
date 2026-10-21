# 5.6.3 Gameplay Performance and Learning Effects

`gameplay_performance.ipynb` examines recognition success and possible adaptation
to the in-game FER model. It supports thesis Section 5.6.3 and the discussion of
recognition-related sampling limitations in Sections 6.1.1 and 8.3.

## Run with Docker

From this directory, run:

```bash
docker compose up --build
```

Open <http://localhost:8888/lab> using the token printed in the terminal.
Open `gameplay_performance.ipynb` and select **Kernel > Restart Kernel and Run All Cells**.
The image provides Python and the packages specified in `requirements.txt`.
The current directory is mounted into the container, so notebook changes and
exports persist. The server is token-protected and accessible through localhost.

To stop, press Ctrl+C, then run `docker compose down`.

## Input and analysis

Place `gameplay-logs.csv` alongside the notebook. Each row contains one logged
FER prediction. Predictions are grouped by participant, level and emoji ID;
a reenactment succeeds if at least one prediction matches the target category.
This measures recognition success, not independently verified expression quality
or the timing of awarded game points.

The 37 participants completed 70 reenactments each: ten per expression category.
The notebook shows session performance, category-specific curves and their
equally weighted average. It compares reenactments 1–3 with 8–10 and 1–5 with
6–10, including participant-level summaries and level context.

## Optional exports

Figures and tables are always displayed in the notebook. All exports are
disabled by default. Enable the required flags in **Setup**, then rerun all cells:

| Flag | Output |
| --- | --- |
| `EXPORT_PDF` | Vector figures in `figures/` for LaTeX. |
| `EXPORT_PNG` | Figures in `figures/` at 300 dpi. |
| `EXPORT_CSV` | Aggregate tables in `results/`. |
| `EXPORT_METADATA` | Input hashes, package versions and validation counts in `results/analysis_metadata.json`. |

Figure filenames are `gameplay_success_session.pdf` or `.png` and
`gameplay_success_<expression>.pdf` or `.png`, with lowercase category names
and `average`. The eight category/average panels are designed for four rows
and two columns in LaTeX; only the final two panels have x-axis labels.

CSV exports include `success_by_presentation.csv`,
`success_rates_by_reenactment.csv`, expression and participant summaries,
`success_by_level.csv`, and environment and validation tables.
All paths are relative to the notebook directory. Output directories are
created only when the corresponding exports are enabled.

## Interpretation

The analysis is descriptive; no significance tests are performed. Changing
difficulty and recognition opportunities, fixed ordering, prior familiarisation
and ceiling effects limit conclusions about learning. Recording-window durations
in the level summary come from thesis Section 5.7, not prediction timestamps.

A lack of consistent improvement does not rule out adaptation. Recognition-based
rewards may discourage difficult-to-recognise expression variants; manual
annotation cannot recover variants that participants never produced.
