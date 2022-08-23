Getting Started
===============

In this practical, we will be going over data preprocessing, tomogram reconstruction, and subtomogram averaging using our TOMOMAN/STOPGAP pipeline. 
A typical cryo-electron tomography (cryo-ET) workflow is illustrated in figure 1. 


.. figure:: images/overview.png
    :width: 800
    
TOMOMAN 
-----------------

TOMOMAN, i.e. TOMOgram MANager, is a MATLAB package for managing the various preprocessing steps for taking raw data to reconstructed tomograms. 
TOMOMAN mainly acts as a set of wrapper scripts for external packages, managing the input and outputs of each external module to form a cohesive pipeline. 
Preprocessing metadata is collected into the TOMOMAN “tomolist” (default name ``tomolist.mat``) while output is written to a plain-text log file (default name: ``tomoman.log``). 
 
External Packages 
--------------------
During this practical we will be using a number of software packages, which have been installed on the BAND platform. These include: 

.. figure:: images/list_of_packages.png
   :width: 800
   

Running TOMOMAN 
--------------------

TOMOMAN can be run directly within MATLAB or as a compiled MATLAB package in a parallel  
environment using SLURM. Since we are doing this practical using EMBL’s BAND platform, the remote desktop’s computational resources are minimal. 
We will be using TOMOMAN within MATLAB to perform tasks that require manual intervention, such as checking input stacks, and TOMOMAN on SLURM for all other tasks.   
 
When running TOMOMAN from MATLAB, it will call external packages from within MATLAB. 
As such, you must first load all the necessary modules into the terminal session prior to starting MATLAB. 
For this practical, we will need to load MATLAB and IMOD prior to starting MATLAB: 
 
- Start a new terminal 
- Load MATLAB and IMOD modules 

::

    module load MATLAB IMOD/4.11.12-binary 
    
- Start matlab 

::

    matlab 
 
 
Setting up the MATLAB Environment 
--------------------

While most of the software required for this practical are set up as LMOD modules, MATLAB scripts and libraries need to be set up on each user’s own MATLAB path. 
Specifically, we’ll need to use the TOMOMAN and STOPGAP toolbox scripts. 
These can be found in the shared scratch folder here: 

``/scratch/subtomo_pratical/MATLAB_scripts/ ``
 
To set up your working directory and copy the MATLAB scripts: 
- Start a new terminal 
- Go to the ``/scratch/`` directory
- Make your own working directory 
- Copy the entire ``MATLAB_scripts/`` directory to your working directory. 
 
 
To add these scripts to your MATLAB path, press the Set Path button on the main window. 
Add the ``MATLAB_scripts/`` folder in your home directory using the ``“Add with Subfolders”`` option. 
Save your ``pathdef.m`` file to your home directory.  
 
Your MATLAB environment is now set up for the practical.  
 
