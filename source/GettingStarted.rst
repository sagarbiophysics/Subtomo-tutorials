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
During this practical we will be using a number of software packages, which have been installed on the BAND platform. These include: 
