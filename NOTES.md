## macOS installation

On macOS, the `MotiFiesta` dependencies require CPU versions (not GPU)
of the PyTorch extensions.

See <https://github.com/pyg-team/pytorch_geometric#additional-libraries>

The following steps are repeatable for installing a macOS (CPU), though
admittedly there are some redundancies.


### Install

```bash
git clone https://github.com/BorgwardtLab/MotiFiesta.git
cd https://github.com/BorgwardtLab/MotiFiesta.git

python3 -m venv venv
source venv/bin/activate
python3 -m pip install -U pip wheel setuptools

python3 -m pip install -U torch torch-geometric
python3 -m pip install torch_scatter torch_sparse -f https://data.pyg.org/whl/torch-2.0.0+cpu.html

python3 -m pip install -r requirements.txt
python3 -m pip install .
./setup.sh

#python3 -m pip uninstall pyg_lib
```


## Datasets

Build the datasets:

```bash
python3 scripts/build_data_motifiesta
```


## Models

Then download from <https://drive.proton.me/urls/BN2X8ZHQAR#UQFR3LELTwhj>
saved locally as `models.tar.gz` and inflate it to obtain the models:

```bash
tar -xzvf models.tar.gz
```


## Training

For the CLI commands, see:

```bash
python3 scripts/motifiesta train -h
```

Example training with the `synth-distort-barbell-d0.05` dataset:

```bash
python3 scripts/motifiesta train -da synth-distort-barbell-d0.05 -n test
```

Currently we're troubleshooting an error in this example:

```bash
Traceback (most recent call last):
  File "/Users/paco/src/MotiFiesta/scripts/motifiesta", line 120, in <module>
    motif_train(model,
  File "/Users/paco/src/MotiFiesta/MotiFiesta/training/train.py", line 254, in motif_train
    mot_loss = model.freq_loss(internals_pos,
  File "/Users/paco/src/MotiFiesta/MotiFiesta/training/model.py", line 304, in freq_loss
    density_neg = self.distance_density(x_pos, x_neg, k=k)
  File "/Users/paco/src/MotiFiesta/MotiFiesta/training/model.py", line 239, in distance_density
    R,_ = knn.query(X.cpu().detach().numpy(), k=k)
  File "sklearn/neighbors/_binary_tree.pxi", line 1123, in sklearn.neighbors._kd_tree.BinaryTree.query
ValueError: k must be less than or equal to the number of training points
```
