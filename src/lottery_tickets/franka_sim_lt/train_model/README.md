# Base policies trained with behavior cloning

Train a flow matching policy with `python train.py dataset.data_path=...`. Evaluate it with `python evaluate.py evaluation.model_path=...`. Configured using hydra, see `cfgs`.

Tested with machine generated demonstrations in FrankaSim, see `examples/demogen`.

