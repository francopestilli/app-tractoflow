# Tractoflow Diffusion MRI Processing Pipeline

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Runs the TractoFlow Nextflow pipeline (scilus 1.6.0 container) on a DWI and T1w image to produce preprocessed DWI, a resampled T1 and a particle-filtering (PFT) tractogram.**

Arranges the inputs as a single-subject TractoFlow input folder, installs Miniconda and Nextflow 21.10.6 at run time, clones the scilus/tractoflow repository and runs it with the scilus 1.6.0 Singularity image. TractoFlow performs DWI preprocessing (denoising, eddy/topup, registration, resampling), tensor and fODF fitting and PFT tracking. It needs internet access on the compute node and about four hours on 16 cores.

| | |
|---|---|
| 📥 **Inputs** | `neuro/dwi` · `neuro/anat/t1w` |
| 📤 **Outputs** | `neuro/dwi` · `neuro/anat/t1w` · `neuro/track/trk` |
| 🐳 **Container** | `scilus/scilus:1.6.0` |
| ⚙️ **Resources** | 16 CPUs · 4 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.868](https://doi.org/10.25663/brainlife.app.868) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` | yes | Raw DWI (NIfTI) with bvals and bvecs. |
| `t1w` | `neuro/anat/t1w` (acpc_aligned) | yes | ACPC-aligned T1-weighted image of the same subject. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `Resample_DWI` | `neuro/dwi` | Preprocessed, resampled DWI from TractoFlow with the bvals/bvecs of the fODF shell. |
| `Resample_T1` | `neuro/anat/t1w` | Preprocessed, resampled T1 from TractoFlow. |
| `PFT_Tracking` | `neuro/track/trk` | Probabilistic particle-filtering tractogram seeded in white matter (.trk). |

<!-- brainlife-app:end -->

An automated, containerized workflow for preprocessing and analyzing diffusion-weighted imaging (DWI) data using Tractoflow. This app manages inputs and outputs following Brainlife.io standards, facilitating reproducibility, portability, and integration into neuroimaging pipelines.

---

## Authors

**Sydney Fulton**
(email:sydney.fulton@utexas.edu [](mailto:sydney.fulton@utexas.edu))

**Gabriele Amorosino**
(email:gabriele.amorosino@utexas.edu [](mailto:gabriele.amorosino@utexas.edu))

---

## Description

This pipeline organizes diffusion MRI data according to Tractoflow requirements, executes preprocessing steps, including eddy current correction, tensor fitting, and anatomical registrations, and outputs processed data ready for subsequent tractography and analysis. The pipeline leverages Singularity containers to ensure reproducibility across different computational environments.

---

## Usage

### Running on Brainlife.io

#### Via Web UI

1. Visit [Brainlife.io](https://brainlife.io) and locate the `app-tractoflow`.
2. Click the **Execute** tab.
3. Upload required inputs
4. Submit the job. Outputs include preprocessed DWI images and associated files suitable for further analyses.

#### Via CLI

1. Install and log into the Brainlife CLI:

   ```bash
   bl login
   ```

2. Run the pipeline:

   ```bash
   bl app run --id <app_id> --project <project_id> \
     --input dwi:<dwi_id> \
     --input bvals:<bval_id> \
     --input bvecs:<bvec_id> \
     --input t1:<t1_id>
   ```

Replace placeholders with the appropriate dataset and project IDs.

---

### Running Locally

#### Option: Using a Configuration File

1. Clone the repository:

   ```bash
   git clone https://github.com/your_username/app-tractoflow.git
   cd app-tractoflow/
   ```

2. Prepare a `config.json` file:

   ```json
   {
       "dwi": "sub-01_dwi.nii.gz",
       "bvals": "sub-01.bval",
       "bvecs": "sub-01.bvec",
       "t1": "sub-01_t1.nii.gz"
   }
   ```

   * Diffusion-weighted imaging file (`.nii.gz`)
   * Associated `.bval` and `.bvec` files
   * T1-weighted anatomical MRI (`.nii.gz`)
   * Optionally, reverse-phase b0, aparc+aseg, or wmparc images

3. Execute the pipeline:

   ```bash
   bash ./main
   ```
### Requirements

* [Singularity](https://sylabs.io/guides/latest/user-guide/)
---

## Outputs

* `data/output/` – Processed DWI images and intermediate outputs
* Tensor fitting results
* Eddy-corrected DWI images
* Anatomical registrations and transformations

---

## Citation

If utilizing this pipeline, please cite:

* Theaud, G., Houde, J. C., Boré, A., Rheault, F., Morency, F., & Descoteaux, M. (2020). TractoFlow: A robust, efficient and reproducible diffusion MRI pipeline leveraging Nextflow & Singularity. Neuroimage, 218, 116889.
* Hayashi, S., Caron, B. A., Heinsfeld, A. S., ... & Pestilli, F. (2024). brainlife. io: a decentralized and open-source cloud platform to support neuroscience research. Nature methods, 21(5), 809-813.
---
