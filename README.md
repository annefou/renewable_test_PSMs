# Test case renewable power system models




## Overview

<img style="float: right;" src="documentation/6_region_diagram.jpg" alt="drawing" width="540" height="450">

This repository contains model files, time series data and example code for a class of simple renewable test case power system models. They serve as "toy" models on which to test conduct renewable energy analysis.

In renewable energy policy and planning, open-source models and data typically represent a real system and may be highly computationally expensive to run. In more pure power system research, the standard test case benchmark models (e.g. the various IEEE X-bus test case systems) are usually not designed for renewable analysis and/or require time series data that is not publicly available. This repository provides some simple "toy" test-case power system models on which to test methodologies, particularly those related to time series and renewable energy analysis. The repository is fully self-contained; all the files required to create the models, 38 years of time series data, and sample code is provided.

The models are designed to be simple "toy" examples (and hence run fast in most settings), but have all the features of "real" power system models. There are two base models. The `1 region` model has only one region in which supply and demand must be met. The `6 region` model has six regions with a transmission topology, and supply and demand must be matched across the model but transmitted between the regions. It is based on a renewable version of the *IEEE 6-bus test system*. See `documentation` for full details on the models.






## How to cite

If you use this repository in your own research, please cite the following paper:

AP Hilbers, DJ Brayshaw, A Gandy (2020). Quantifying demand and weather uncertainty in power system models using the *m* out of *n* bootstrap. [arXiv:1912.10326](https://arxiv.org/abs/1912.10326).




## Usage

The easiest way to start using these models is by modifying the example script provided. The file `main.py` contains three example runs with different specs.
- `1_region_plan_LP`: run the `1 region` model in planning mode. For baseload technology, any nonnegative capacity is allowed and there is no ramping constraint. This makes the optimisation problem a continuous linear program.
- `6_region_plan_MILP`: run the `6 region` model in planning mode. Baseload technology may only be installed in integer units of 3GW, and has a ramping constraint of 20%/hr. This makes the optimisation problem a mixed-integer linear program.
- `6_region_operate`: run the `6 region` model in operate mode. Baseload technology has a ramping constraint. The installed capacities of the generation and transmission capacities across the model are defined in `models/6_region/model.yaml`, and only the generation levels are determined by the model.

Each of these examples can be run from a unix command line via

```
python3 main.py --run_name $RUN_NAME
```

where `$RUN_NAME` is either `1_region_plan_LP`, `6_region_plan_MILP` or `6_region_operate`.

A run may be customised by creating a custom `run_dict` and `ts_data` in `main.py`. Runs may give various warnings from the `Calliope` backend. 




## Contains

### Modelling & data files
- `models/`: the files that define the power system model, in the open-source modelling framework `Calliope` (see acknowledgements)
- `data/demand_wind.csv`: the full 38-year demand and wind time series data for the `6 region` model. For the `1 region` model, a specific demand and wind columns must be chosen. By default these are both taken from `region_5`.


### Code
- `main.py`: example runs of the models
- `models.py`: the models as Python classes, along with some helper functions
- `tests.py`: tests used in the development of the model. These tests can be modified and may be helpful if you want to create customised versions of these models.


### Miscellaneous
- `documentation/`: detailed documentation on the models
- `benchmarks/`: benchmark outputs used in the tests. Helpful for debugging or when customising models.




## Requirements & Installation

Since `main.py` is a short file with only a few functions, it's probably easier to directly copy-paste any relevant code into a personal project as opposed to installing a new module. For this reason, this repository does not contain a `setup.py` file.

Running `main.py` requires:
- Python modules:
  - `Calliope 0.6.4`:  see [this link](https://calliope.readthedocs.io/en/stable/user/installation.html) for installation.
  - `numpy 1.62.2`
  - `pandas 0.24.2`
- Other:
  - `gurobi`: a solver, used to solve the optimisation problems. It is not open-source but is free with an academic license. An alternative is `cbc` (see [this link](https://projects.coin-or.org/Cbc)), which is fully open-source. The solver can be specified in `models/{MODEL_NAME}/model.yaml`.





## Contact

Adriaan Hilbers. Department of Mathematics, Imperial College London. [a.hilbers17@imperial.ac.uk](mailto:a.hilbers17@imperial.ac.uk).




## Acknowledgements

Models are constructed in the modelling framework `Calliope`, created by Stefan Pfenninger and Bryn Pickering. See [callio.pe](https://callio.pe) or the following paper for details:

- Pfenninger, S. and Pickering, B. (2018). Calliope: a multi-scale energy systems modelling framework. Journal of Open Source Software, 3(29), 825, doi:[10.21105/joss.00825](https://doi.org/10.21105/joss.00825).

The demand and wind dataset is based on work by Hannah Bloomfield et al. Details can be found in the following paper:

- Bloomfield, H. C., Brayshaw, D. J. and Charlton-Perez, A. (2019) Characterising the winter meteorological drivers of the European electricity system using Targeted Circulation Types. Meteorological Applications. ISSN 1469-8080 (in press). doi:[10.1002/met.1858](https://doi.org/10.1002/met.1858)




- Data does not have leap days
- Code written for easy development of models -- use tests to check model behaves as desired.
- Warnings of type `monetary` interest rate of zero for technology XXXX, setting depreciation rate as 1/lifetime. can be ignored.
