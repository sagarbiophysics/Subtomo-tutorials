Introduction
============

In this session, we will work on the HIV immature capsid dataset (EMPIAR-10164). 
Due to the limitations of the BAND platform, we will mainly be processing one tomogram. 
The goals for this session include:

- Setting up TOMOMAN parameter files
- Performing preprocessing using TOMOMAN
- Submitting TOMOMAN jobs to SLURM
- Manual fiducial-based tomogram alignment in IMOD
- 3D-CTF corrected tomogram reconstruction with NovaCTF
- Spherical particle picking in Chimera using the Pick Particle tool
- Oversampling initial particle positions on spherical surfaces 
- Setting up a STOPGAP directory, motivelist, and wedgelist
- Submitting STOPGAP jobs to SLURM
- Extracting subtomograms using STOPGAP
- Subtomogram averaging in STOPGAP
- Visualizing particle positions in Chimera using the Place Objects tool
- Generating a low-resolution de novostructure suing a small subset of particles
- Expanding the dataset

Test Dataset
----------------

The test data we will be using for this practical is tilt series TS_01 from the EMPIAR-10164 dataset. 
Copy the entire `/scratch/subtomo_practical/HIV_practical/` directory to your own working directory. 
This directory contains a `tomo/` folder, with `frames/` and `rawdata/` subfolders. 
The `frames/subfolder` contains the `raw .mrc` frames for `TS_01`, wile the `rawdata/` folder contains the SerialEM `.mdoc` file for this tilt series. 


.. note::
     The data contained in this folder are just symbolic links; this is to minimize the time required for copying the raw data and extra use of storage space.
