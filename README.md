<!-- Copyright (c) Rutav Shah, Indian Institute of Technlogy Kharagpur  -->
<!-- Copyright (c) Facebook, Inc. and its affiliates -->
## Quick Links
[Wesbite](https://sites.google.com/view/abstractions4rl)   |   [Paper](https://arxiv.org/abs/2107.03380)   |   [Video](https://youtu.be/Yj1EHvGWmRA)

# RRL: Resnet as representation for Reinforcement Learning
Resnet as representation for Reinforcement Learning (RRL) is a simple yet effective approach for training behaviors directly from visual inputs. We demonstrate that features learned by standard image classification models are general towards different task, robust to visual distractors, and when used in conjunction with standard Imitation Learning or Reinforcement Learning pipelines can efficiently acquire behaviors directly from proprioceptive inputs.

Final Behaviors acquired using RRL on [ADROIT benchmark](https://sites.google.com/view/deeprl-dexterous-manipulation) tasks (left to right) (a) Opening a door (b) Hammering a nail  (c) Pen-twirling (d)) Object relocation
![All Tasks](examples/results.gif?raw=flase)


## Setup
`RRL` codebase can be installed by cloning this repository. Note that it uses git submodules to resolve dependencies. Please follow the steps as below to install correctly.

1. Clone this repository along with the submodules
    ```
    git clone --recursive https://github.com/facebookresearch/RRL.git
    ```
2. Install the package using `conda`. The dependencies (apart from `mujoco_py`) are listed in `env.yml`
    ```
    conda env create -f env.yml

    conda activate rrl
    ```
3. The environment require MuJoCo as a dependency. You may need to obtain a license and follow the setup instructions for mujoco_py. Setting up mujoco_py with GPU support is highly recommended.

4. Install `mj_envs` and `mjrl` repositories.
    ```
    cd RRL
    pip install -e mjrl/.
    pip install -e mj_envs/.
    pip install -e .
    ```
5. Additionally, it requires the demonstrations published by [`hand_dapg`](https://github.com/aravindr93/hand_dapg/)

## Running Instructions
1. First step is to convert the observations of demonstrations provided by [`hand_dapg`](https://github.com/aravindr93/hand_dapg/) to the encoder feature space. An example script is provided [here](https://github.com/facebookresearch/RRL/blob/main/examples/convertDemos.py). **Note** the script saves the demonstrations in a `.pickle` format inside the `rrl/demonstrations` directory.

    For the `mj_envs` tasks :
    ```
    python convertDemos.py --env_name hammer-v0 --encoder_type resnet34 -c top -d <path-to-the-demo-file>
    ```
    ```
    python convertDemos.py --env_name door-v0 --encoder_type resnet34 -c top -d <path-to-the-demo-file>
    ```
    ```
    python convertDemos.py --env_name pen-v0 --encoder_type resnet34 -c vil_camera -d <path-to-the-demo-file>
    ```
    ```
    python convertDemos.py --env_name relocate-v0 --encoder_type resnet34 -c cam1 -c cam2 -c cam3 -d <path-to-the-demo-file>
    ```
2. Launching `RRL` experiments using [DAPG](https://sites.google.com/view/deeprl-dexterous-manipulation).

     An example launching script is provided [`job_script.py`](https://github.com/facebookresearch/RRL/blob/main/examples/job_script.py) in the `examples/` directory and the configs used are stored in the `examples/config/` directory. **Note** : Hydra configs are used.
    ```
    python job_script.py  demo_file=<path-to-new-demo-file> --config-name hammer_dapg
    ```
    ```
    python job_script.py  demo_file=<path-to-new-demo-file> --config-name door_dapg
    ```
    ```
    python job_script.py  demo_file=<path-to-new-demo-file> --config-name pen_dapg
    ```
    ```
    python job_script.py  demo_file=<path-to-new-demo-file> --config-name relocate_dapg
    ```
