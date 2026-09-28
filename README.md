# Few-shot Bioacoustic Event Detection

Code for a DCASE few-shot bioacoustic event detection project. It extracts PCEN audio features and trains a prototypical network with a ResNet encoder. Evaluation supports threshold sweeps, optional transductive refinement, and post-processing.

## Setup

Create an environment and install the dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

The audio and annotations are not included. Before running the code, set `path.root_dir`, `path.train_dir`, and `path.eval_dir` in `config.yaml` for your clone and dataset. The committed defaults point to the original workstation; keep your local paths out of commits. You can also pass these values as Hydra command-line overrides.

## Run

Run each stage from the repository root.

Extract features:

```bash
python main.py set.features=true set.train=false set.eval=false
```

Train the model:

```bash
python main.py set.features=false set.train=true set.eval=false
```

Evaluate with the configured threshold:

```bash
python main.py set.features=false set.train=false set.eval=true eval.sweep=false
```

Sweep thresholds:

```bash
python main.py set.features=false set.train=false set.eval=true \
  eval.sweep=true eval.sweep_start=0.4 eval.sweep_stop=0.8 eval.sweep_step=0.05
```

## Results

Feature files and model checkpoints are written under `Features/` and `Model/`. Evaluation outputs, training metrics, and a copy of the run configuration are saved under `outputs/`.

The current configuration uses the ResNet encoder. The `scripts/` directory contains helpers for evaluation and parameter sweeps. DCASE scoring code is in `evaluation_metrics/`.

## Files

- `fsbio/` contains feature, data, model, sampler, and metric code.
- `main.py` runs feature extraction, training, or evaluation.
- `config.yaml` holds the run settings and local data paths.
- `scripts/` contains sweep helpers.
- `evaluation_metrics/` contains the DCASE scoring implementation.
