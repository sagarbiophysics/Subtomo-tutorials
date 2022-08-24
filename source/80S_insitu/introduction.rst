Introduction
===============

In this session, we will work on the *in situ* 80S yeast Ribosome dataset (EMPIAR-XXXXX). 
Due to the limitations of the BAND platform, we will mainly be working with one tomogram. 
However, we will use already preprocessed and picked particles to for subtomogram averaging and classification.

The goals for this session include:

- Setting up TOMOMAN parameter files
- Performing preprocessing using TOMOMAN for ``EER`` data
- Automated fiducial-less alignment in ``AreTomo``
- Setting up and running ``Template matching`` using STOPGAP
- Subtomogram averaging in STOPGAP
- Setting up simulated annealing multireference classification in STOPGAP

.. note::
    This section assumes basic working knowledge of TOMOMAN.
    Make sure you followed through the ``HIV`` practical.


Test Dataset
----------------

The test data we will be using for this practical is tilt series Position_01 from the EMPIAR-XXXXX dataset. 
Copy the entire ``/scratch/subtomo_practical/80S_practical/tomoman_single/`` directory to your own working directory. 
This directory contains a ``19102021/`` folder, with ``frames/``, ``raw_data/``, and ``raw_data_single`` subfolders. 
The ``frames/`` subfolder contains the ``raw.eer`` EER frames, wile the ``raw_data_single/`` folder contains the Tomo5 ``.mdoc`` files for a simngle tilt series we will use for the practical.

.. note::
     The data contained in this folder are just symbolic links; this is to minimize the time required for copying the raw data and extra use of storage space.
