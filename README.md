# PharmaDiff: Pharmacophore-Conditioned Diffusion Model for De Novo Drug Design

[Link to the paper](https://www.arxiv.org/abs/2505.10545)

Authors: Amira Alakhdar, Barnabas Poczos, Newell Washburn


The authors of this code would like to thank the authors of [MiDi](https://arxiv.org/abs/2302.09048) for making their code publicly available, which served as a valuable reference for this work.

## Installation

This code was tested with PyTorch 2.0.1, cuda 11.8 and torch_geometric 2.3.1 on multiple gpus.

  - Download anaconda/miniconda if needed
  - Create a rdkit environment that directly contains rdkit:
    
    ```conda create -c conda-forge -n pdiff rdkit=2023.03.2 python=3.9```
  - `conda activate pdiff`
    
  - Install the nvcc drivers for your cuda version. For example:
    
    ```conda install -c "nvidia/label/cuda-11.8.0" cuda```
  - Install a corresponding version of pytorch, for example: 
    
    ```pip3 install torch==2.0.1 --index-url https://download.pytorch.org/whl/cu118```
  - Install other packages using the requirement file: 
    
    ```pip install -r requirements.txt```

  - Run:
    
    ```pip install -e .```



## Datasets

  - QM9 should download automatically
  - For GEOM, download the data  from this [link](https://drive.google.com/file/d/1ZdIXiINLmRD6MnbnCKkjvRxwZN8rrutH/view?usp=sharing) and put in `PharmaDiff/data/geom/raw/`:

## Training:

This model was derived from the adaptive MiDi model with no hydrogens, with adjustments to fit pharmacore-conditioned generation, for training you need to inside the `pharmadiff` folder  (so that the outputs are saved at the right location). 

For QM9 without hydrogens:

``` python3 main.py dataset=qm9 dataset.remove_h=True +experiment=qm9_no_h_adaptive```

GEOM-DRUGS without hydrogens:

``` python3 main.py dataset=geom dataset.remove_h=True +experiment=geom_no_h_adaptive```


## Resuming a previous run

  Run:

``` python3 main.py dataset=geom dataset.remove_h=True +experiment=geom_no_h_adaptive general.resume='path_to checkpoint' ```


## Evaluation
Run:

``` python3 main.py dataset=geom dataset.remove_h=True +experiment=geom_no_h_adaptive general.test_only='path_to checkpoint' ```


## Checkpoints

QM9:
  - command: `python3 main.py dataset=qm9 dataset.remove_h=True +experiment=qm9_no_h_adaptive`
  - checkpoint: https://drive.google.com/file/d/1dnaMjC2BukL2or1Ur75-QddpKA9YIp_5/view?usp=sharing



Geom:
  - command: `python3 main.py dataset=geom dataset.remove_h=True +experiment=geom_no_h_adaptive`
  - checkpoint: https://drive.google.com/file/d/1MV6wZnfNYeJem_xKGbKXZ6ECrF0aenmd/view?usp=sharing





## Use PharmaDiff with an input pharmacophore

- To use PharmaDiff with a pharmacophore input, provide the pharmacophore as a .pkl file. To generate this file, you will need an SDF file containing either:
  - the atoms/fragments associated with the pharmacophore
  - the full reference molecule.

A step-by-step tutorial on generating the .pkl file will be provided in the examples/pharmacophore_prep.ipynb jupiter notebook.

- Then the .pkl can be used as 

``` python3 main.py dataset=geom dataset.remove_h=True +experiment=geom_no_h_adaptive general.test_only='path_to_checkpoints' general.sample_condition='path_to_pkl' ```

- The number of generated molecules per condition can be adjusted from general.test_sampling_num_per_graph

