TOMOMAN setup 
=============

In this section, we will go over the process of setting up the TOMOMAN workflow.


Initializing TOMOMAN Parameter files
---------------

In this step, we will copy a set of TOMOMAN parameter files to the tomogram folder. 
These parameter files are plain-text files that tell TOMOMAN which processing step you wish to run and the parameters for that step. 



#. In MATLAB, go to the `tomo/` folder. 
#. Copy empty parameter files, using the current directory as the `root_dir`, and using default filenames TOMOMAN filenames.

:: 
   
   tomoman_copy_paramfiles([pwd,'/']);

Parameter files for all TOMOMAN tasks are now copied into the `root_dir`. 
While files for all tasks are copied, they aren't necessarily all used, so you can delete them as you wish. 


Sorting New Stacks
---------------

The first step in TOMOMAN processing is sorting new data into directories for each tilt series. 
During this step, TOMOMAN scans a `raw_stack_dir` for `.mdoc` files. 
For each one it finds, it generates a new folder to contain all the preprocessing data, links the .mdoc file, creates a frames/ subdirectory, parses frame names from the .mdoc file, and generates links for all frames from the raw_frame_dir to the frames/ subdirectory. 
Sorting new data is performed using the tomoman_sortnew.param file. 
Open this file in the MATLAB editor by double clicking it. 
Types of parameters are typically broken into comment blocks.

Sorting new data is performed using the tomoman_sortnew.param file. 
Open this file in the MATLAB editor by double clicking it. 
Types of parameters are typically broken into comment blocks.

1.	The directory parameter block contains information about working directories. 
   The root_dir should already be set from copying. 
   For this dataset, the raw_stack_dir and raw_frame_dir should be rawdata/ and frames/, respectively. 
2.	The tomolist block contains filenames for TOMOMAN’s output files. 
   This should already be set during copying.
3.	The filename parameters are for the raw stacks generated during data collection. 
   For this dataset, we don’t have them so this can be set to none.
4.	The data collection block contains information specific to the parameters used for data collection and the setup of the microscope. 
   This dataset contains gain-normalized .mrc files, so set the gainref parameter to none. 
   All other defaults are fine.  
5.	The override .mdoc values comment block allows users to override fields that are normally parsed from the .mdoc file. 
   This may be important when certain settings aren’t properly calibrated in SerialEM.
   For this dataset, set the tilt_axis_angle to 85.3, the dose_rate to 8, and the pixelsize to 1.35. target_defocus will be parsed from the .mdoc file; this is typically the best choice when varying the defocus during batch tilt series acquisition, so this field should be set when collecting data.  
6.	The final block is the sorting parameters, which allows you to ignore certain missing files. 
   Here raw stacks refer to tilt series image stacks generated during data collection; these are typically just non-motion corrected summed frame stacks, so they can be safely ignored. 
   TOMOMAN also allows you to ignore missing frames, though this is not recommended.  
7.	After setting your parameters, run the TOMOMAN sortnew task:

   ::
      
      tomoman([pwd,'/'],'tomoman_sortnew.param');

The tilt series folder should now be properly set. 

.. note::
   the sortnew task can be repeatedly run and only new data will be sorted. This can be useful if you wish to process data during your data acquisition. 


Preparing to Run TOMOMAN on SLURM
---------------

To run TOMOMAN on SLURM, we use a bash script to manage the cluster parameters. 

#.	Copy the bash script from /scratch/subtomo_practical/SLURM_scripts/run_tomoman_slurm.sh to the tomogram root_dir.  
#.	Open the bash script. The run options block sets the SLURM job settings. 
   The default settings are appropriate for running a GPU task for this practical.
#.	The directories field has the parameters for the root_dir and the TOMOMAN parameter file to run. 
   Set the root_dir to the tomogram directory. 



