Template matching
===============

In this section we will pick 80S Ribosome particles in ``bin8_aretomo`` tomographic volumes we reconstructed using TOMOMAN. 
For this demonstration we will work with a single tomogram ``ay19102021_rid_3_atc_lamella1_position1_dose-filt.rec``.

The goals for this section include:

- Setting up a STOPGAP template matching folder.
- Preparing tomographic volumes for template matching and generating tomogram volume masks.
- Generating a template from PDB model.
- Generating an angle list defining angular search used for template matching. 
- Preparing necessary ``lists`` for template matching run.
- Running a template matching job on SLURM.

Preparing a STOPGAP folder
-----------------


Here we will initialize a subtomogram averaging folder with the necessary directory structure. 

1. In your ``80S_practical`` directory, make ``tm/bin8_single/`` subdirectory. 
Change into the ``bin8_single/`` directory. 
 
2. Load the STOPGAP module.
 
3. Run the initialize folder command with the subtomo task:

::
     
     $ stopgap_initialize_folder.sh tm
 
4. The folder now has the required structure for subtomogram averaging jobs. 
Re-running ``stopgap_intialize_folder.sh`` for other jobs will add the additional required folders without affecting old ones.
 
5. From the ``/scratch/subtomo_pratical/SLURM_scripts/`` folder, copy ``run_stopgap.sh`` and ``stopgap_tm_parser.sh``. 

Generating Volume masks for Tomograms and tomogram list
-----------------

Next, we will create a tomogram folder ``tomos`` inside the template matching directory ``bin8_single``

1. Create a symbolic link to 



Generating the angle list
-----------------


Generating a template list
-----------------



Generating a template matching parameter file
-----------------



Submitting temnplate matching job on SLURM
-----------------
