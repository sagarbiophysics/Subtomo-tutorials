Introduction
===============

In this session, we will work on the *in situ* 80S yeast Ribosome dataset (EMPIAR-XXXXX). 
Due to the limitations of the BAND platform, we will mainly be working with one tomogram. 
However, we will use already preprocessed and picked particles to for subtomogram averaging and classification.

The goals for this session include:

- Setting up TOMOMAN parameter files
- Performing preprocessing using TOMOMAN for ``EER`` data
- Automated fiducial-less alignment in ``AreTomo``
- Template matching using STOPGAP
- Subtomogram averaging in STOPGAP
- Setting up simulated annealing multireference classification in STOPGAP


Test Dataset
----------------

The test data we will be using for this practical is tilt series Position_01 from the EMPIAR-XXXXX dataset. 
Copy the entire ``/scratch/subtomo_practical/80S_practical/`` directory to your own working directory. 
This directory contains a ``tomo/`` folder, with ``frames/`` and ``rawdata/`` subfolders. 
The ``frames/subfolder`` contains the ``raw .mrc`` frames for ``TS_01``, wile the ``rawdata/`` folder contains the SerialEM ``.mdoc`` file for this tilt series. 


.. note::
     The data contained in this folder are just symbolic links; this is to minimize the time required for copying the raw data and extra use of storage space.
