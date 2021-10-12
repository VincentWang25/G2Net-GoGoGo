# Reproduce 1D Model Result

Below you can find a outline of how to reproduce our 1D Model solution for the G2Net-Gravitational-Wave-Detection competition.

## HARDWARE: (The following specs were used to create the original solution)

1 x NVIDIA Tesla A100 or 2 x NVIDIA GeForce RTX 2080Ti.

## Below are the shell commands used in each step, as run from current directory

1. Download kaggle competition data (assume the Kaggle API is installed)
   1. `cd ../data/`
   2. `kaggle competitions download -c g2net-gravitational-wave-detection`
   3. `unzip -q g2net-gravitational-wave-detection`
2. Generate whiten wave from competition data
   1. run notebook `1D_Model/notebooks/generate_whiten_wave.ipynb)`
3. Reproduce Result
   1. `cd ../1D_Model/`
   2. reproduce models:`./reproduce_train.sh`  
   3. reproduce predictions: `./reproduce_infer.sh`

## MODEL BUILD: There are three options to produce the solution.

1) very fast prediction: run `notebook/stacking.ipynb`
    1) runs in 2 hours
    2) uses precomputed predictions

2. ordinary prediction: run `reproduce_infer_test.sh`
   1. expect this to run for 6 hours
   2. uses pretrained models

3. retrain models: run `python reproduce_train.sh` 
   1. expect this to run about a week
   2. trains all models from scratch
   3. If we want to use ensemble, we need to run `python reproduce_infer_oof_test.sh`. Then we need to change the directory inside `notebook/stacking.ipynb` correspondingly to point to each oof prediction and submission file.



## Folder CONTENTS

- notebooks/:
  - `stacking.ipynb`: for stacking, use hacking-lb method
  - `SyntheticSignal.ipynb`: for GW simulation
  - `get_avg_w0.ipynb`: for avg_w0 generation
  - `generate_whiten_wave.ipynb`: for whiten wave generation: `../data/1D_Model/whiten-train-w0/` and `../data/1D_Model/whiten-test-w0/`
  - Richard_Models/: folder contains the original notebook for model generation from Richard
- src/:
  - `augmentation.py`: augmentation functions 
  - `config.py`: Model configuration
  - `dataset.py`: dataset preparation
  - `infer_helper.py`: helper functions for inference
  - `loss.py`: related loss functions
  - `lrfinder.py`: learning rate finder class
  - `models.py`: interface to decide which model to use
  - `models_1d.py`: 1D model structure
  - `models_2d.py`: 2D model structure
  - `models_3d.py`: 3D model structure
  - `optim.py`: optimizer class
  - `train_helper.py`: helper functions for training
  - `TTA.py`: class for test time augmentation
  - `util.py`: utility functions
- `infer.py`: inference interface
- `train.py`: training interface
- `reproduce_trian.sh`: script file for model reproduction
- `reproduce_infer.sh`: script file for model prediction (OOF, test) reproduction
