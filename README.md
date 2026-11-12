# README
Author: Simon Sacher
Date: 12/11/2026
Version: 1
Project: Foundation Model Pipeline

# Process
## set up python venv with uv
> uv init
> uv venv
> uv add -r pangea/pangea-bench/requirements.txt

## add more
> uv add huggingface # will supply `hf` on cmd?

### Notes
- export UV_HTTP_TIMEOUT=60
- MIGHT HAVE TO SWITCH TO CONDA for nvidia??

### later just sync with given pyproject.toml and uv.lock file
> uv sync

## Pangea
> git clone https://github.com/VMarsocci/pangaea-bench.git


### datasets
- pastis-r: supplied automatically via pangea
- biomassters: supplied via Huggingface. TODO!! (ca. 200GB -> leave for proper )
    > hf download nascetti-a/BioMassters --repo-type dataset


