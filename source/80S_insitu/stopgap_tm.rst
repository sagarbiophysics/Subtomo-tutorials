Template matching
===============

In this section we will pick 80S Ribosome particles in ``bin8_aretomo`` tomographic volumes we reconstructed using TOMOMAN. 
For this demonstration we will work with a single tomogram ``ay19102021_rid_3_atc_lamella1_position1_dose-filt.rec``.

The goals for this section include:

- Setting up a STOPGAP template matching folder.
- Preparing tomographic volumes for template matching and generating tomogram volume masks.
- Generating a ``template`` from PDB model.
- Generating an ``angle list`` defining angular search used for template matching. 
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

Next, we will create a tomogram folder ``tomos`` inside the template matching directory ``bin8_single``. 

1. Create a symbolic link to the reconstructed tomogram in ``tomoman_single/novactf_bin8``.

2. Open tomogram using ``3dmod`` and generate a boundary model. 
Save the model as ``tomo_name.mod``.

3. Copy and execute bash script ``model2point_all.sh`` from ``subtomo_practical/template_matching_scripts/``.

4. Open ``matlab`` and navigate to ``tomos`` folder. 
Open and edit ``creat_boundarymask.m`` for ``tomo_dir``.
``padding`` defines number of pixels to pad along the XY edge of the tomogram. 
Leave it as ``10``.
run `` creat_boundarymask.m``.

5. Move into ``bin8_single/lists`` folder. 
Copy and edit script ``sg_tm_generate_tomo_list.m`` for appropriate paths.
Run the script. 


6. Move into ``tomoman_single`` folder where you preprocessed tilt-series and generate a wedgelist:

::
     
     sg_wedgelist_from_tomolist('tomolist.mat','wedgelist.star');
 
8. Copy the list into the lists/ subfolder in your STOPGAP template matching directory. 


Generating the angle list
-----------------

1. Open ``sg_tm_generate_anglist.m``

2. ``anginc`` parameter defines the angular sampling on the unit sphere.
For 80S ribosmes, ``12`` or ``15`` degrees sampling is often sufficient. 
Smaller exposures result in larger number of angles STOPGAP will search during template matching. 
We will usr ``15`` degrees to speed up computing. 

3. you can specify the symmetry of the template you are searching using ``sym``. 
For 80S Ribosome, it is set to ``c1``.

4. Finally, set ``output_name`` to reflect ``anginc`` and ``sym``. 
Here, we will set it to ``anglist_15deg_c1.csv``.

Preparing a template using a PDB model
-----------------

Most often you will not have a map for the particle you want to pick in the tomographic volume. 
For this tutorial we will generate a simulated map from PDB model for 80S Ribosome. 

1. Download ``mmCIF`` format model for PDBID ``6gqv``.

2. Open it using ``ChimeraX`` and save it as ``.pdb`` formatted file. 

3. We will use ``simulate`` program from ``cisTEM`` package to generate a 3d map. 
Execute ``simulate`` and follow through command pront.

4. Open generated map using ``3dmod``. 
One can see that the contrast of this map is inverted compared to the tomographic volume. 

5. We will use ``relion_image_handler`` to invert the contrast. 

::


6. Next, we want to bin this map ``8x`` in order to use it for template matching on ``8x`` binned tomogram. 
We will use ``IMOD's`` ``binvol`` program to bin the simulated volume by the factor of 8. 


Generating a template list
-----------------

Next, we should generate a template list for STOPGAP template matching. 

1. open ``sg_tm_generate_


Generating a template matching parameter file
-----------------



Submitting temnplate matching job on SLURM
-----------------








Extracting particle positions
-----------------


