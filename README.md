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
> cd THOR
#### installing THOR to the venv
> uv pip install . 


## Pytest
run pytest in root project directory once to test everything working
> cd $FOUNDATION-MODEL-DIRECTORY
> pytest

### NOTE
- will fail on datasets that cant be automatically downloaded
- >module load gcc