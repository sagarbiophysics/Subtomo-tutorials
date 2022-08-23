Template matching
===============


Preparing a STOPGAP folder
-----------------


Here we will initialize a subtomogram averaging folder with the necessary directory structure. 

1. In your ``80S_practical`` directory, make ``tm/init_bin8/`` subdirectory. 
Change into the ``init_bin8/`` directory. 
 
2. Load the STOPGAP module.
 
3. Run the initialize folder command with the subtomo task:

::
     
     $ stopgap_initialize_folder.sh tm
 
4. The folder now has the required structure for subtomogram averaging jobs. 
Re-running ``stopgap_intialize_folder.sh`` for other jobs will add the additional required folders without affecting old ones.
 
5. From the ``/scratch/subtomo_pratical/SLURM_scripts/`` folder, copy ``run_stopgap.sh`` and ``stopgap_tm_parser.sh``. 

Generating Volume asks for Tomograms and tomogram list
-----------------



Generating the angle list
-----------------


Generating a template list
-----------------



Generating a template matching parameter file
-----------------



Submitting temnplate matching job on SLURM
-----------------
