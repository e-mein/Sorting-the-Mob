README
Sorting the mob: using geometric morphometrics and machine learning to differentiate kangaroo tarsals for zooarchaeological applications
Author: Erin Mein
Analysis script metadata date: 9 November 2022

OVERVIEW
--------
This repository contains R code for geometric morphometric and statistical
analyses of large macropod astragali and calcanea. The script covers landmark-coordinate
import and preparation, Generalised Procrustes Analysis (GPA), principal
components analysis (PCA), between-group PCA, allometric analyses, visualisation
of shape differences using reference meshes, and genus-level classification
using linear discriminant analysis (LDA), k-nearest neighbours (kNN), random
forest, and linear support vector machine models.

The script is an analysis workflow rather than a fully automated software
package. Some steps depend on the supplied data, specimen ordering, and
manually specified specimen indices. Check the notes below before running it.

REPOSITORY CONTENTS
-------------------
The analysis script expects the following files and folders relative to the
repository root:

  Calcaneus/Data/
    Specifier.csv
      Specimen metadata used to associate each calcaneus specimen with its genus,
      species, specimen ID, age, sex, and other recorded variables.

    Coordinates/
      One .txt coordinate file per specimen.

	Reference surface meshes used in the shape-visualisation sections.
    		M_gig_QM-JM20893.ply
    		O_ruf_SAM-M16383.ply
    		O_rob_WAM-M12450.ply
    		N_agi_WAM-M11620.ply
    		W_bic_UQ98.ply

  Astragalus/Data/
    Specifier.csv
      Specimen metadata used to associate each astragalus specimen with its genus,
      species, specimen ID, age, sex, and other recorded variables.

    Coordinates/
      One .txt coordinate file per specimen.

	Reference surface meshes of astragali used in the shape-visualisation sections.
    		M_gig_QM-JM20891.ply
    		N_agi_MAGNT-U0237.ply
    		O_ruf_SAM-M27398.ply
    		W_bic_UQ98.ply
   
The script refers to the mesh files by name. Confirm that each file is
present in the expected location before running the relevant sections.
If the repository uses a different folder layout or filenames, update the
relative paths in the script accordingly.

REQUIREMENTS
------------
R is required. The code uses the following R packages:

  geomorph
  rgl
  Morpho
  plyr
  tidyverse
  caret
  nlme
  doParallel
  gdata
  ggbeeswarm


GETTING STARTED
---------------
1. Clone or download this repository.
2. Open the project in R or RStudio, using the repository's top-level folder
   as the working directory.
3. Check that the Data folder and its expected files are present.
4. Install the required R packages listed above.
5. Open the analysis script and follow the sections in order, starting with
   data import and landmark preparation.
6. Review the checks described below before interpreting the results.

FILE PATHS AND WORKING DIRECTORY
--------------------------------
The R scripts use relative file paths to access the data within the repository. 
No setwd() commands or user-specific absolute file paths are required.

These paths assume that the R working directory is the repository root (Sorting-the-Mob/).

If the working directory is not the repository root, set it to the appropriate location 
before running the script. The scripts themselves do not change the working directory.

DATA IMPORT AND SPECIMEN ORDER
------------------------------
The script reads specimen metadata from Data/Specifier.csv and coordinate
files from Data/Coordinates. The coordinate array and metadata must refer to
the same specimens in the same order for downstream analyses to be valid.

Before running the analyses:
- Confirm that each row in Specifier.csv corresponds to the intended
  coordinate file.
- Confirm that the coordinate files are read in the same order as the rows
  in Specifier.csv. The script includes a commented check using specimen
  names; enable and inspect an equivalent check if needed.
- Confirm that coordinate files use the expected Morphologika format and
  contain compatible landmark data.
- Confirm that the landmark numbering matches the script's assumptions.
- Check that the reference mesh files correspond to the specimens named in
  the script.

The script removes landmarks 9 and 30 and defines sliding semilandmarks using
landmark indices. Do not change these indices unless the landmark scheme and
analysis are being intentionally revised.

ANALYSIS WORKFLOW
-----------------
The main sections of the script are:

1. Import specimen metadata and landmark coordinates.
2. Remove designated landmarks and define sliding semilandmark curves.
3. Run Generalised Procrustes Analysis (GPA).
4. Explore centroid size, PCA, and between-group PCA.
5. Test allometric relationships and compare allometric slopes.
6. Visualise shape differences by warping reference meshes.
7. Fit and assess genus-classification models:
   - Linear discriminant analysis (LDA)
   - k-nearest neighbours (kNN)
   - Random forest
   - Linear support vector machine (SVM)

The classification sections use caret and leave-one-out cross-validation
(LOOCV). Random seeds are set in the script for relevant model runs. Model
tuning can be computationally intensive, particularly when testing many
principal-component counts or running parallel processing.

IMPORTANT SETTINGS TO CHECK
----------------------------
Several parts of the script use manually specified specimen positions or
model settings. These are tied to the dataset and should not be changed
casually.

Examples include:
- Selecting specimen ranges for genus-specific analyses.
- Selecting the number of principal components and tuning parameters in the
  final classification models.
- Selecting a particular reference mesh and coordinate-array position for
  mesh-warping visualisations.

If the metadata rows or coordinate-file order change, these indices may no
longer refer to the intended specimens. Verify them against the metadata
before running the relevant sections. 

RUNNING THE SCRIPT
------------------
Run the script sequentially, section by section, in a fresh R session. The
workflow creates objects that are reused by later sections, so running
isolated sections without first creating their required objects may fail.

The script includes interactive 3D visualisations and optional mesh-warping
movie commands. Some output is intended to be inspected interactively rather
than saved automatically. Movie-generation commands are commented out in the
provided script; enable them only after checking the output folder and
required inputs.

TROUBLESHOOTING
---------------
- "cannot open file" or file-not-found errors: check that the working
  directory is the repository root and that the expected files exist.
- Missing function errors: install and load the package providing the
  function, and check that the package version is compatible with your R
  version.
- 3D graphics errors: check rgl installation and your system's graphics
  support.
- Parallel-processing errors: try reducing the worker count or running the
  affected section without parallel processing.
- Unexpected plots or classification results: recheck specimen ordering,
  metadata alignment, landmark numbering, manually specified specimen ranges,
  and model settings.

REPRODUCIBILITY AND LIMITATIONS
-------------------------------
This repository documents the analysis code used for the study; it is not a
turnkey, cross-platform pipeline guaranteed to run without adaptation.
Users should inspect the checks above and report any changes made to paths, 
packages, data, or analysis settings when reproducing the workflow.

LICENCE
-------
Unless otherwise stated, the analysis code in this repository is released
under the Creative Commons Attribution 4.0 International (CC BY 4.0) licence.
You may share and adapt the code for any purpose, including commercially,
provided you give appropriate credit to the author, provide a link to the
licence, and indicate whether changes were made. You may do so in any
reasonable manner, but not in a way that suggests the author endorses you or
your use.

Licence details: https://creativecommons.org/licenses/by/4.0/

Suggested attribution:
Mein, E., Manne, T., Veth, P. and Weisbecker, V. (2026). Sorting the Mob. [https://github.com/e-mein/Sorting-the-Mob]. Licensed under
CC BY 4.0.

CITATION
--------
If you use this code in published work, please cite the associated
publication and acknowledge the repository. 

CONTACT
-------
Erin Mein
