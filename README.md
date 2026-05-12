# Code and Data for: "Native habitat affinities predict fish invasions with post-invasion habitat shifts"

[![DOI](https://img.shields.io/badge/DOI-Pending-blue.svg)](https://doi.org/10.1111/1365-2656.XXXXX)

This repository contains the complete analytical pipeline, data, and R scripts required to reproduce the analyses, statistical models, and figures presented in the *Journal of Animal Ecology* manuscript. The codebase is structured sequentially into distinct stages to facilitate reproducibility and transparency, from initial data processing to the final ensemble predictions of invasion risk.

---

## 📁 Core Data File
**File:** `sp_level_data.csv`

This is the primary dataset used across all analyses. To maintain absolute statistical integrity, the dataset represents a unified, strictly filtered pool of 179 fish populations across the Red Sea and the Mediterranean Sea.

**Key variables included in this dataset:**
* **Taxonomy & Metadata:** Species name, family, basin (Red Sea or Mediterranean), and invasion status.
* **Assemblage Groups:** Categorization into one of the four analyzed groups (Red Sea Natives, Red Sea Lessepsians, Mediterranean Natives, Mediterranean Lessepsians).
* **Demographics:** Local abundance metrics derived from stereo-BRUVs (e.g., mean MaxN).
* **Depth:** Depth limits and ranges (e.g., minimum depth, depth range).
* **Habitat Affinities:** High-resolution, independent habitat associations (e.g., rocks, corals, sand, macroalgae, artificial structures). These are calculated as the proportion of deployments in which a specific habitat feature was present across all samples where the population appeared.

---

## ⚙️ Analytical Pipeline
The code is divided into sequential stages. Each RMarkdown file represents a distinct analytical step in the manuscript's framework.

### Stage 1: Data Preparation and Exploration
* **Script:** `stage_1_data_prep.Rmd`
* **Function:** Loads the raw, sample-level stereo-BRUV data and aggregates it to generate the core species-population level dataset (`sp_level_data.csv`).
* **Details:** This script handles the dynamic multicollinearity filtering pipeline. It computes Pearson correlation matrices and verifies the statistical independence of the final retained ecological predictors. It also generates the supplementary trait distribution visualizations.

### Supplemental Stage 1: Sample Independence Analysis
* **Script:** `supplemental_stage_1_samples_distance.Rmd`
* **Function:** Validates the spatial independence of simultaneous stereo-BRUV deployments.
* **Details:** Calculates the exact horizontal spatial distances between overlapping simultaneous camera deployments using GPS coordinates. Generates summary statistics alongside a density distribution plot, formally demonstrating that deployments were placed far enough apart to ensure independent sampling.

### Stage 2: Multidimensional Ecological Space & Post-Invasion Shifts
* **Script:** `stage_2_trait_space.Rmd`
* **Function:** Evaluates the overarching ecological space occupied by the distinct assemblages and tracks species-specific habitat shifts.
* **Details:** * Computes Gower distance matrices and constructs a Principal Coordinates Analysis (PCoA) based on environmental, distributional, and habitat metrics (**Figure 3**).
  * Maps multidimensional, species-specific habitat shifts from the native Red Sea baseline to the invaded Mediterranean realization using vector arrows.
  * Performs Procrustes rotation analyses to statistically test the magnitude of post-invasion habitat lability.

### Stage 3.1: Predictive Modeling - Linear Discriminant Analysis (LDA)
* **Script:** `stage_3_1_predict_nis_lda.Rmd`
* **Function:** Implements the multivariate classification component of our predictive framework.
* **Details:** * Runs separate LDAs structured around our "three-comparison framework" (Native Range, Native-Invaded Range, Invaded Range).
  * Uses Out-of-sample Leave-One-Out Cross-Validation (LOOCV) AUC to empirically quantify the discriminatory performance of selected variables.
  * Extracts posterior class-membership probabilities and variable loadings to generate density plots (**Figure 4**).

### Stage 3.2: Predictive Modeling - Generalized Linear Mixed Models (GLMM)
* **Script:** `stage_3_2_predict_nis_glmm.Rmd`
* **Function:** Implements the probabilistic assessment of introduction potential while accounting for taxonomic relatedness.
* **Details:** * Uses a multi-model inference approach (`dredge`) based on AICc to identify the most parsimonious combination of independent ecological predictors.
  * Calculates relative variable importance (Sum of Akaike Weights) to prevent subjective selection.
  * Computes out-of-sample predictive performance using LOOCV AUC and determines explanatory power via hierarchical variance partitioning (`glmm.hp`).
  * Validates model assumptions using `DHARMa` residual diagnostics and Variance Inflation Factors (VIF).

### Stage 4: Ensemble Predictions and Candidate Species
* **Script:** `stage_4_ensample_cand.Rmd`
* **Function:** Combines the outputs from Stage 3.1 (LDA) and Stage 3.2 (GLMM) into a rigorous, cross-validated ensemble prediction index to forecast future invaders.
* **Details:** * Integrates predicted probabilities into a performance-weighted ensemble index (weighted by LOOCV AUC scores).
  * Identifies the optimal risk threshold via ROC analysis (maximizing Youden’s J statistic).
  * Identifies specific "high-risk" native Red Sea species that display ecological pre-adaptations of successful Lessepsian migrants.
  * Generates final consensus rankings, the ROC curve (**Figure S5**), and plots highlighting candidate species for proactive management (**Figure 5**).

---

## 💻 System Requirements & Reproducibility

**R Environment:**
All scripts were written in R. 

**Key Packages:** * `MuMIn` (for multi-model inference)
* `DHARMa` (for residual diagnostics)
* `glmm.hp` (for variance partitioning)
* `vegan` (for PCoA)

**How to run:**
To reproduce these analyses, please clone this repository:
`git clone https://github.com/shahar710/JAE-2025-01071.git`

Then, open the **`code_for_submission.Rproj`** file in RStudio. Running the scripts from within this R Project environment ensures that all relative file paths for data loading, model saving, and plot generation work automatically.

---

## 📝 Citation
If you use the data or code in this repository, please cite the original publication:

> Chaikin, S., Gavriel, T., Deveto, A., Malamud, S., & Belmaker, J. (2024). Native habitat affinities predict fish invasions with post-invasion habitat shifts. *Journal of Animal Ecology*. [DOI Link]

## 📜 License
This project is licensed under the MIT License - see the LICENSE file for details.
