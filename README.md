# PINNs for Laminar Flow in a Sudden-Constriction Channel

A mixed-variable physics-informed neural network (PINN) for steady, two-dimensional incompressible laminar flow through a channel with a sudden constriction. The model predicts velocity, pressure, and stress fields, compares them with ANSYS Fluent reference solutions, and computes wall shear stress.

This repository accompanies the bachelor's thesis [*Experience in Using Neural Networks for Calculating Two-Dimensional Flows of Viscous Fluid*](https://elib.spbstu.ru/dl/3/2023/vr/vr23-4068.pdf/info). A copy of the thesis is included in [`VKR_TroshinOf.pdf`](./VKR_TroshinOf.pdf).

![Sudden-constriction channel geometry](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/82229eeb-5600-483c-9786-e47ed4fb0780)

## What is implemented

- A TensorFlow PINN for the steady incompressible Navier-Stokes equations.
- A mixed-variable formulation based on streamfunction, pressure, and stress components.
- Physics, inlet, outlet, no-slip wall, and optional Fluent-data loss terms.
- Latin hypercube sampling for collocation and boundary points.
- Experiments for Reynolds numbers `10`, `14`, `50`, `75`, `100`, `250`, and `500`.
- Visual comparison of PINN and Fluent velocity/pressure fields.
- Wall-shear-stress calculation and export.
- English and Russian versions of the main notebook.

## Model formulation

For a steady incompressible flow, the network predicts

$$
(\psi, p, \sigma_{11}, \sigma_{22}, \sigma_{12}),
$$

where the velocity is derived from the streamfunction:

$$
u = \frac{\partial\psi}{\partial y},
\qquad
v = -\frac{\partial\psi}{\partial x}.
$$

This construction satisfies continuity by definition. The stress tensor is

$$
\boldsymbol{\sigma}
= -p\mathbf{I} + \mu\left(\nabla\mathbf{v} + \nabla\mathbf{v}^{T}\right),
$$

and the momentum residual is evaluated as

$$
\rho(\mathbf{v}\cdot\nabla)\mathbf{v} - \nabla\cdot\boldsymbol{\sigma} = 0.
$$

The total objective combines the PDE residual, boundary conditions, and an optional supervised term built from Fluent data.

## Repository contents

| Path | Purpose |
| --- | --- |
| [`PINN_Step_With_Data_Small_Example.ipynb`](./PINN_Step_With_Data_Small_Example.ipynb) | English notebook with model definition, training, evaluation, and plots. |
| [`PINN_Step_With_Data_Small_Example_RU.ipynb`](./PINN_Step_With_Data_Small_Example_RU.ipynb) | Russian version of the notebook. |
| [`step/`](./step/) | Fluent reference fields for the available Reynolds numbers. |
| [`saved_models/`](./saved_models/) | Selected trained models and exported results. The `Re=500_iters=50_000(bad)` run is retained as an explicitly unsuccessful experiment. |
| [`VKR_TroshinOf.pdf`](./VKR_TroshinOf.pdf) | Full methodology, experiments, and discussion. |

## Run in Google Colab

- [English small example](https://colab.research.google.com/drive/1qPDsg42_trAny4ys3qoh6veoRogL5U-O?usp=sharing)
- [Russian small example](https://colab.research.google.com/drive/1FCG6tH6hDtPLaqkrDkuEUES4DPCiDBD5?usp=sharing)
- [Full experiment notebook](https://colab.research.google.com/drive/11i08nuDCRhlHF0NKjtO6cfDjtOh0Va7I?usp=sharing)

The committed notebooks are Colab-oriented and contain `gdown`, `unzip`, and archive commands. Review those cells before running locally.

## Local environment

The notebooks require Python with the following packages:

```text
tensorflow
tensorflow-addons
numpy
scipy
pandas
matplotlib
seaborn
pyDOE
ipywidgets
```

TensorFlow Addons supports only specific TensorFlow versions. Use a compatible pair and record the resolved versions before a long training run.

## Running an experiment

1. Open either the English or Russian notebook.
2. Choose one execution path: initialize a new network or load a saved model. Do not run both setup branches unchanged.
3. Select a Fluent reference file from `step/Re=<value>/FluentSolStep.txt`. The current preprocessing cell expects a file named `FluentSolStep.txt` in the notebook working directory; copy the selected file there or update the cell.
4. Set viscosity, loss weights, collocation counts, network size, and training iterations for the selected Reynolds number.
5. Run the setup and training cells in order. Start with a reduced iteration count to verify the environment and paths.
6. Run the result cells to compare PINN and Fluent fields and export wall shear stress.

Typical outputs include:

- `uvp_step.png` with velocity and pressure comparisons;
- `wall_shear_stress.png` and `wall_shear_stress.txt`;
- serialized model files;
- loss histories and training plots.

![PINN and Fluent field comparison](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/23ed764b-f30b-41e4-9ba4-88b0531b8912)

## References

- C. Rao, H. Sun, and Y. Liu, [*Physics-informed deep learning for incompressible laminar flows*](https://arxiv.org/abs/2002.10558), 2020.
- [Original TensorFlow 1 implementation](https://github.com/Raocp/PINN-laminar-flow).
- Mikhail S. Troshin, *Experience in Using Neural Networks for Calculating Two-Dimensional Flows of Viscous Fluid*, bachelor's thesis, Peter the Great St. Petersburg Polytechnic University, 2023.

## Architecture

![PINN architecture](https://github.com/sleshworld/PINNs-Laminar-Flow-Sudden-Construction-Channel/assets/36676116/f7ce7014-2b9e-4d37-9a71-9ea708ba8004)
