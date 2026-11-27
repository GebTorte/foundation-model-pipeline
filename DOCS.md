# Troubleshooting History
## PASTIS HD
- root_path changed to ./data/pastis/
- Place PASTIS under pangea/pangaea-bench/data/pastis/ or override dataset.root_path
- The required layout includes metadata.geojson, ANNOTATIONS/, DATA_S2/, DATA_S1ASC/, and DATA_SPOT/
- PASTIS is not downloaded by the Hydra constructor; download_url is null and auto_download is False

## DOFA + BioMassters: missing wavelength for `CLP`

**Date:** 2026-11-23

### Symptom

Running:

```bash
torchrun --nnodes=1 --nproc_per_node=1 pangaea/pangaea-bench/pangaea/run.py --config-name=train dataset=biomassters encoder=dofa decoder=reg_upernet_mt_ltae preprocessing=reg_default criterion=mse task=regression

torchrun --nnodes=1 --nproc_per_node=1 pangaea/run.py --config-name=train dataset=biomassters encoder=dofa decoder=reg_upernet_mt_ltae preprocessing=reg_default criterion=mse task=regression
```

ended with:

```text
ConfigKeyError('Missing key CLP
    full_key: optical.CLP
    object_type=dict')
```

after downloading `./pretrained_models/DOFA_ViT_base_e100.pth`.

### Cause

The BioMassters dataset configuration lists `CLP` as an optical input band:

```yaml
bands:
  optical:
    - ...
    - CLP
```

DOFA dynamically maps every requested input band to a wavelength through `pangaea/encoders/dofa_encoder.py`:

```python
self.wv_list = [
    self.wave_list[m][bi] for m, b in self.input_bands.items() for bi in b
]
```

The DOFA encoder's `wave_list` does not contain `CLP`, so Hydra construction fails before training starts. This is a band/config mismatch, not a failure of the pretrained-weight download.

### Resolution

For spectral imaging with DOFA, remove the auxiliary `CLP` band from `configs/dataset/biomassters.yaml` and remove its corresponding value from the optical `data_mean`, `data_std`, `data_min`, and `data_max` lists.

Alternatively, add `CLP` with an explicit wavelength to the DOFA `wave_list`:

```yaml
wave_list:
  optical:
    CLP: 0.55
```

Adding a pseudo-wavelength is only appropriate if `CLP` is intentionally used as a spectral-like input. For normal Sentinel-2 DOFA usage, removing it and keeping the ten physical Sentinel-2 bands is the safer approach.

The downloaded DOFA weight file is reusable; rerun after correcting the band configuration.


## BioMassters dataset
DL via huggingface dl tool

### combine and unzip zips
cd to dir

recombine train zip and unpack afterwards
> cat train_features.z{01..13} train_features.zip > train_features_combined.zip && unzip train_features_combined.zip
(partly bad headers but most files should be fine)

recombine test zip and unpack afterwards
> cat test_features_splits.z{01..04} test_features_splits.zip > test_features_splits_combined.zip && unzip test_features_splits_combined.zip

unpack agbm
> unzip train_adbm
> tar xvf test_adbm.tar

### mv train_features up after unzip
mv Volumes/Samsung_T5/BIOMASS/BioMasster_Dataset/v1/DrivenData/train_features/ .

### create train,test,val _Data_list.csv from metadata.csv.csv
```python
import polars as pl
df = pl.read_csv("The_BioMassters_-_features_metadata.csv.csv")
train_df = df.filter(
    pl.col("split") == "train"
)
test_df = df.filter(
    pl.col("split") == "test"
)

train_df.write_csv("train_Data_list.csv")
test_df.write_csv("test_Data_list.csv")

# then move them to biomassters folder
```
NOTE: val has not been split yet