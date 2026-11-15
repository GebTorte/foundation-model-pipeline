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


### Notes
- export UV_HTTP_TIMEOUT=60
- MIGHT HAVE TO SWITCH TO CONDA for nvidia??

### later just sync with given pyproject.toml and uv.lock file
> uv sync

### activate venv
> source .venv/bin/activate

## Pangea
> git clone https://github.com/VMarsocci/pangaea-bench.git



### build as package
> cd pangea-bench
> uv pip install --no-build-isolation --no-deps -e .

### datasets
- pastis-r: supplied automatically via pangea
OR 
    > hf download IGNF/PASTIS-HD --repo-type dataset
- biomassters: supplied via Huggingface. TODO [ ] (ca. 200GB -> leave for proper )
    > hf download nascetti-a/BioMassters --repo-type dataset

### test run
> torchrun --nnodes=2 --nproc_per_node=1 pangea/run.py --config-name=train dataset=croptypemappi
ng encoder=ssl4eo_dino decoder=seg_upernet_mt_ltae preprocessing=seg_resize criterion=cross_entropy task=segmentation

### test pastis TODO [ ]
> torchrun --nnodes=1 --nproc_per_node=1 pangaea/run.py --config-name=train dataset=pastis encoder=remoteclip decoder=seg_upernet_mt_ltae preprocessing=seg_resize criterion=cross_entropy task.evaluator.inference_mode=whole task=segmentation

## THOR
> git clone

### add thor.py file to pangea/encoders to interface THOR framework
- gpt did well ?!?

### TODO install thor into 
THOR repo not available in the current Python environment; using a compatibility adapter. Install the THOR package or add the repo to PYTHONPATH. Original error: No module named 'thor'

#### installing THOR to the venv
> cd THOR
> uv pip install . 

### Dataset support
#### Thor dataset V2
"S2-10m",
"S2-20m",
"S2-60m",
"S1-10m",


## Pytest
run pytest in root project directory once to test everything working
> cd $FOUNDATION-MODEL-DIRECTORY
> pytest

### NOTE
- will fail on datasets that cant be automatically downloaded?!
- > module load gcc


# Setup repository

> git clone https...fmp.git
> git submodule update --init # updates thor and pangaea

> git submodule foreach "uv pip install --no-build-isolation --no-deps -e ."

## UV version (not tested)
> git clone https...fmp.git
> uv add git+https://github.com/GebTorte/THOR.git
> uv add "pangaea @ git+https://github.com/GebTorte/pangaea-bench.git"

Let uv handle everything.

