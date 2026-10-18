Sections to modify:
5.
5.6 Evaluation of EmojiHeroVR
5.6.3 Gameplay Performance and Learning Effectss

6.1 Data Annotation and Filtering
6.1.1 Selection of Annotation Candidates

8.3 Limitations and Conditions of Interpretation



# 5.6.3 Gameplay Performance

Analyse the reconstructed EmojiHeroVR gameplay logs for the thesis minor correction
concerning possible adaptation to the in-game FER model.

`gameplay_performance.ipynb` is the complete analysis. It counts an emoji as
successful when at least one logged FER prediction matches its prompted category.
Each emoji contributes one binary outcome; predictions are not treated as separate
reenactment outcomes. This is an operational measure of recognition success,
not independent verification of expression quality or an exact reconstruction of
the timing of awarded game points.

## Run with Docker

From this directory, run:

```bash
docker compose up --build
```

Open `http://localhost:8888/lab` and use the token printed by Jupyter in the terminal.
Open `gameplay_performance.ipynb`, then select **Kernel > Restart Kernel and Run All Cells**.
The directory is mounted into the container, so notebook changes and exports persist.
Stop with Ctrl+C, then run `docker compose down` to remove the stopped container.

The image uses Python **3.14.8**, a maintained Python release, on Debian slim.
The base image digest and every Python dependency are pinned. JupyterLab, pandas,
NumPy and Matplotlib are the main packages. Installation uses binary wheels;
no GPU runtime, compilers or additional analysis services are required.
The server remains token-protected and its published port is bound to localhost.

The container runs as user ID 1000. On Linux, a host account with a different user
ID may need ownership/permissions adjusted on this directory for notebook saving.

## Run in an existing Python environment

Use Python 3.14 and install the same pinned requirements:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

For headless execution from this directory:

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=180 gameplay_performance.ipynb
```

## Input and outputs

The supplied CSV is in `data/gameplay-logs.csv`. If using the repository without the
local analysis package, supply that file first; see `data/README.md`.
Input CSV files are ignored by Git, and the Docker build context includes only
the Dockerfile and requirements.

The notebook creates:

- `figures/gameplay_success_by_expression.pdf` and `.png`: the seven-panel figure.
- `figures/gameplay_success_<expression>.pdf`: seven separate category figures.
- `results/success_by_presentation.csv`: the values in the plots, with numerators and denominators.
- `results/success_by_expression.csv`: category totals and descriptive first-three versus last-three comparisons.
- `results/success_by_level.csv`: level totals and prediction-count context.
- `results/analysis_metadata.json`: input fingerprint, data-quality counts and installed versions.

Only aggregate results are displayed or exported. The full reenactment-level table
remains an in-memory notebook object.

## Scope and interpretation

Each of the 37 participants completed 70 emojis: ten per expression category.
The seven curves show success for successive presentations within each category.
Dashed lines separate the first three presentations (Level 1 or 2, depending on
category), the next three (Level 3), and the final four (Level 4).

This is a descriptive inspection of within-session performance. No significance
tests, regression models, smoothing or per-prediction learning analyses are used.
The first-three versus last-three comparison summarises the curves and is not
an estimate of a learning effect isolated from changing game difficulty.

Later levels have shorter recording windows and fewer recognition opportunities,
particularly Level 4. Performance is also affected by fixed ordering, prior
familiarisation and ceiling effects. The notebook retains unsuccessful emojis,
counts invalid requests as non-matching, and does not use timestamps to order
reenactments. It verifies the supplied success flags against the logged classes.
No clear improvement would not establish the absence of adaptation, and manual
annotation cannot recover expression variants participants never produced.

The recorded window durations quoted in the notebook are contextual values from
thesis Section 5.7; they are not inferred from prediction timestamps.
