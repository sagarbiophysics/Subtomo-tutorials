Tilt-series preprocessing
============

Making Motion-Corrected Stacks
----------------

The tilt series directory has been set up, but before we can examine it, we must first perform motion correction on the frames and assemble the motion corrected images into a stack. 
For this dataset, we will be using MotionCor2. 

1. The tomolist parameters should already be correctly set.  

2. The TOMOMAN parameters field set parameters for how TOMOMAN runs. 
Most tasks have some “force” parameter, which tells TOMOMAN to repeat the task on tilt series that have already been processed. 
If set off, TOMOMAN only runs the task on tilt series that have not yet been processed. 

3. This dataset was collected on a K2 in super-resolution mode. 
The image_size parameter sets the output image size; in this case we want a normal resolution output image. 
Set image_size to 3712,3712; this pads a K2 image by 2 pixels in one axis, but results in an image size with amenable to binning at factors of 2. 
 
4. The MotionCor2 block contains MotionCor2’s parameters. 
For this dataset, most of the defaults are fine, but be sure to set the FtBin parameter to 2 so that the aligned frames are Fourier cropped back to normal size. 
The Gpu parameter can be ignored when running on SLURM.
 
5. Since this dataset has .mrc frames, the EER block can be ignored. 
We will not be doing noise2noise training, so odd/even stacks are not needed.
 
6. Submit this TOMOMAN job to SLURM. First, make sure the bash file has the correct paramfilename. 
Then, in a terminal, run the bash script. 

.. note::
     the SLURM jobs does not return any output to the terminal. To monitor progress, use the squeue function and check the log files. 


Cleaning Stacks
----------------

After generating your motion-corrected tilt series stacks, you now have something you can reasonably look at to assess the quality of the images. 
Bad images can include images where something blocks the field of view, such as a grid bar or ice crystal, or bad imaging conditions such as sample charging. 
The best way to assess this is by manually looking at the images. TOMOMAN facilitates the process using the clean_stacks task. 

1. Open tomoman_clean_stacks.param. 
The tomolist parameters block should be set up already. 
 
2. For the stack cleaning parameters block, clean_binning refers to the binning factor for displaying the tilt series, clean_append is whether to save the cleaned stack with an appended name, and force_cleaning forces repeating of stacks that have already been cleaned. 
The default parameters should work find. 
 
3. Run tomoman_clean_stacks in MATLAB.
Follow the instructions in the command window to clean bad images. 

.. note::
      Remember to close your 3dmod windows; TOMOMAN is unable to close spawned windows from external packages. 

Dose Filtering
----------------

We find that dose filtering (a.k.a. exposure filtering) greatly improves the contrast, alignment quality, and high-resolution signals of reconstructed tomograms. TOMOMAN has its own function for doing this based off the empirical values determined by Grant and Grigorieff (doi: 10.7554/eLife.06980). 

1. Open tomoman_dosefilter.param. 
The tomolist parameter block should already be set correctly. 
 
2. The dose filtering parameter block is usually fine with the default values. 
Pre-exposure is typically from tasks such as mapping and realigning during data collection, but is generally quite low and can be ignored. 
TOMOMAN lets you dose filter frames, if you first generate an aligned frame stack. 
This can better preserve high resolution signals, but for the practical we will just filter the aligned images. 
Critical exposure constants can be provided, but these are typically not known a priori, so we generally use the default values determined by Grant and Grigorieff. 
 
3. Run dose filtering in SLURM.
