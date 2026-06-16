# AGENTS.md

This repository is a collection of standalone data-science / ML **Jupyter notebooks**
(originally authored for Google Colab). There is no server, database, or build step —
the "application" is JupyterLab plus the scientific Python stack.

Notebooks:
- `CIS545FinalProject.ipynb` — EDA + sklearn models on the US Accidents dataset (pandas, geopandas, sklearn).
- `Transformer_Exercise.ipynb` — implement GPT-2 from scratch (torch, transformers, einops).
- `WAFChallenge.ipynb` — TF-IDF + KMeans clustering on a YouTube videos CSV (sklearn).

## Cursor Cloud specific instructions

- Dependencies are installed with `pip install --user -r requirements.txt` (handled by the
  startup update script). Packages land in `~/.local`.
- The Jupyter CLIs (`jupyter`, `jupyter-lab`) live in `~/.local/bin`, which is **not on PATH**
  by default. Prefix commands with `export PATH="$HOME/.local/bin:$PATH"` or invoke via
  `python3 -m jupyterlab` / `python3 -m jupyter`.
- Run JupyterLab with:
  `jupyter lab --no-browser --ip=0.0.0.0 --port=8888 --ServerApp.token=devtoken`
  then open `http://localhost:8888/lab?token=devtoken`.
- Execute a notebook headlessly (good for CI-style checks / quick validation):
  `jupyter nbconvert --to notebook --execute --ExecutePreprocessor.timeout=120 <file>.ipynb`
- `requirements.txt` intentionally **omits** the Colab-only pieces that cannot run outside
  Colab: `google.colab`, `PyDrive`, `oauth2client`, `google_drive_downloader`, and the GitHub
  research packages `easy_transformer` / `pysvelte`. Cells that import those (used only to pull
  input CSVs from Google Drive, or for the interpretability visualizations) will fail locally;
  this is expected. Supply the input data files locally to run the data-dependent cells.
- The notebooks target newer library versions than Colab pinned; some legacy calls (e.g.
  `from sklearn.externals.six import StringIO` in `CIS545FinalProject.ipynb`) are removed in
  current sklearn and will error. This is a notebook code issue, not an environment problem.
- No GPU is available; PyTorch is the CPU build (`torch==*+cpu`). The Transformer notebook runs
  on CPU but slowly.
