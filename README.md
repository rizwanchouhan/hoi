# Learning Interaction Dynamics for Generalizable 3D Human-Object Motion Generation

<img style="max-width: 100%;" src="https://github.com/rizwanchouhan/relimat/blob/main/resources/wax.png" alt="Title Overview">
<img style="max-width: 100%;" src="https://github.com/rizwanchouhan/hoi/blob/main/assets/qualitative.png" alt="InterAct Overview">

## About the Project

We propose an interaction-invariant generative framework for 3D human-object interaction that separates interaction semantics from motion variations. The framework combines an interaction-invariant field, counterfactual interaction learning, and hierarchical motion generation to produce diverse, realistic, and contact-consistent interactions.

## 🔥 Architecture

The overview of the proposed methodology.

<img style="max-width: 100%;" src="https://github.com/rizwanchouhan/hoi/blob/main/assets/overview.png" alt="InterAct Pipeline Overview">

## Installation

1. **Clone the Repository**: Download the project from GitHub.
2. **Set Up Conda Environment**: Create a Conda environment named interact with Python 3.8:
    ```bash
    conda create -n interact python=3.8
    conda activate interact
    pip install torch==2.0.0 torchvision==0.15.1 torchaudio==2.0.1 --index-url https://download.pytorch.org/whl/cu118
    ```
3. **Install PyTorch3D**: Follow the official instructions: [PyTorch3D](https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md).
4. **Install Dependencies**: Install the remaining packages using pip:
    ```bash
    pip install -r requirements.txt
    python -m spacy download en_core_web_sm
    bash install_human_body_prior.sh
    ```
5. **Download Models**: Download SMPL+H, SMPL-X, and DMPL models, place them under `./models/`, and follow [smplx tools](https://github.com/vchoutas/smplx/blob/main/tools/README.md#merging-smpl-h-and-mano-parameters) to merge SMPL-H and MANO parameters.

## Datasets

The project consolidates the following diverse HOI datasets:

- **GRAB Dataset**: A dataset of whole-body human grasping of objects captured with markerless motion capture. [GRAB License](https://grab.is.tuebingen.mpg.de/license.html)
- **BEHAVE Dataset**: A dataset and method for tracking human-object interactions in RGB videos. [BEHAVE License](https://virtualhumans.mpi-inf.mpg.de/behave/license.html)
- **InterCap Dataset**: Joint markerless 3D tracking of humans and objects in interaction from multi-view RGB-D images. [InterCap License](https://intercap.is.tue.mpg.de/license.html)
- **OMOMO Dataset**: Object motion guided human motion synthesis with text annotations. [OMOMO Dataset](https://github.com/lijiaman/omomo_release)
- **NeuralDome / IMHD / CHAIRS Datasets**: Redistributed corrected and augmented HOI data, available upon authorization via the [access form](https://docs.google.com/forms/d/e/1FAIpQLScMCfdd8BXzDBZ3iw0x5zA3KSTlD1F2GTaO8ylDG9Cj1upaPw/viewform?usp=sharing).
- **ARCTIC Dataset**: A dataset for dexterous bimanual hand-object manipulation. [ARCTIC License](https://github.com/zc-alexfan/arctic/blob/master/LICENSE)
- **ParaHome Dataset**: Parameterizing everyday home activities towards 3D generative modeling of human-object interactions. [ParaHome License](https://github.com/snuvclab/ParaHome?tab=readme-ov-file#license)

## Steps for Training

1. **Download Datasets**: Fill out the [access form](https://docs.google.com/forms/d/e/1FAIpQLScMCfdd8BXzDBZ3iw0x5zA3KSTlD1F2GTaO8ylDG9Cj1upaPw/viewform?usp=sharing) to request non-commercial access, or download the datasets from the provided links above and agree to their licenses.
2. **Data Processing**: Run the processing scripts for each dataset (`process/process_behave.py`, `process/process_grab.py`, `process/process_intercap.py`, `process/process_omomo.py`, `process/process_parahome.py`, `process/process_arctic.py`), followed by canonicalization, text segmentation, motion representation extraction, and BPS processing as described in the full README.
3. **Initiate Training**: Execute one of the following to begin the training process for the desired HOI generative task:
    ```bash
    # Text2Interaction
    cd text2interaction
    python -m train.hoi_diff --save_dir ./save/t2m_interact --dataset interact

    # Object2Human
    cd object2human
    bash ./scripts/Train_markerContact_VecDist.sh

    # Human2Object
    cd human2object
    bash ./scripts/train.sh
    ```

## Running the Demo

Follow these steps to run the demo of the project:

1. **Download Pre-trained Model**: Download the pretrained model checkpoints from the [link](https://drive.google.com/file/d/1vfskohWxr7gBuve1MLD1RlGut_xSN8mL/view?usp=sharing) and put them in `./text2interaction/save/`.
2. **Download Evaluator Checkpoints**: Download the checkpoints of the pretrained evaluator and text encoder from the [link](https://drive.google.com/file/d/1-bpafRyaVHdX4TsltDHiGIxcjw-k1Fnf/view?usp=sharing) and put them in `./text2interaction/assets/eval`.
3. **Inference with Contact Guidance**: Run `bash ./scripts/run_sample_guide_contact.sh` for generating interactions with contact guidance.
4. **Inference without Contact Guidance**: Run `bash ./scripts/run_sample_nonguide.sh` for generating interactions without guidance.
5. **Visualization**: Run `python visualization/visualize.py [dataset_name]` to visualize the dataset sequences.

## Citation

If you find this repository useful for your work, please cite:

```bibtex
@inproceedings{xu2025interact,
    title     = {{InterAct}: Advancing Large-Scale Versatile 3D Human-Object Interaction Generation},
    author    = {Xu, Sirui and Li, Dongting and Zhang, Yucheng and Xu, Xiyan and Long, Qi and Wang, Ziyin and Lu, Yunzhi and Dong, Shuchang and Jiang, Hezi and Gupta, Akshat and Wang, Yu-Xiong and Gui, Liang-Yan},
    booktitle = {CVPR},
    year      = {2025},
}
```
