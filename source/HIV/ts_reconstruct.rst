Tomogram reconstruction
============

Tomogram Generation
----------------

While we won’t be using this tomogram for subtomogram averaging, there are a few alignment parameters that can be refined here. 
The main ones are tomogram thickness and Z shift. 
While the boundary model is mostly correct, that accounted for X axis tilt. 
Since we zeroed this out, the ``thickness`` and ``Z shift`` parameters are off; the amount of error depends on how much X axis tilt was present. 

1.	Generate tomogram and open in ``3dmod``. 
Check to see that the specimen is centered in Z and that the entire specimen is within the tomogram boundaries. 
 
2.	If the specimen is not centered in Z, note the shift in pixels. 
Multiply this number by the binning factor and apply this to the ``Z shift`` parameter. 
Re-generate the tomogram and check again.
 
3.	If the specimen is not within the boundaries of the tomogram, increase the ``thickness`` until there is sufficient padding above and below the specimen.

.. note::
     while excessive padding doesn’t affect data quality, it does result in a larger tomogram which will impact the amount of storage space required, the speed of reading the tomogram, and data processing times. 
 
4.	If your specimen has low contrast for picking, you can also use the SIRT-like filter here. 
For this dataset, this is not required.
 
5.	When you are satisfied with your tomogram, delete intermediate image stacks. 

.. note:: 
    this removes the erased, aligned stack. 
    So if you want to come back to this step, you will need to remake this stack in the Final Aligned Stack step. 



Post-processing
----------------

In this step, you will have the options to trim (crop) the tomogram dimensions and rescale the data. 
In general, I don’t recommend cropping; this helps with keeping consistent XY dimensions in the dataset. 
I also don’t recommend converting to bytes, as this can result in banding patterns in the tomogram. 
The only thing I do in this step is to rotate about the X axis, which properly orients how the tomogram is stored on disk. 



Clean Up
----------------

This step is to remove any unnecessary intermediate files. 
Feel free to delete all intermediate files; they will not be necessary for subsequent steps. 



CTF-Estimation
----------------

CTF-estimation for tomography is similar to single particle images, in that Thon ring fitting is used in both. 
However, since tilted images have a defocus gradient, different parts of the images have different Thon rings.
 TOMOMAN includes ``tiltctf``, an algorithm we developed to use tilt series alignment parameters to generate power spectra that account for this defocus gradient. 
 We then give this power spectrum to ``CTFFIND4`` for Thon ring fitting. 

1.	Return to MATLAB and open the ``tomoman_tiltctf.param`` file. 
The directory parameters should already be correct. 
 
2.	The tiltctf parameters include the parameters for calculating power spectra. 
In general, a ``ps_size`` of ``512`` is sufficient and a 0.05 um defocus tolerance is sufficient Defocus tolerance is the maximum allowed tolerance when deciding how to tile tilted images, but tiltctf also always uses a minimum tile overlap of ½ the power spectrum size, so increasing this number may not directly affect computation time. 
 
3.	Fourier scaling should be used if the data is collected for high-resolution work (e.g. pixel size smaller than ~2 Å /pix). 
This helps with potential aliasing in the power spectrum. 
For this dataset, a ``Fourier scaling`` of 2 means the Nyquist frequency in the power spectra will be 5.4 Å. 
In practice, this is not a problem as tilt series data is collected with such low dose per image that Thon rings cannot be fit to very high resolutions.
 
4.	``Defocus range`` is a TOMOMAN parameter that defines the search range that ``CTFFIND4`` will use.
TOMOMAN uses the defocus value stored in the tomolist and provides +/- the defocus range to CTFFIND4. 
If you know your target defocus was incorrect during data acquisition, you can increase this range.
 
5.	The ``CTFFIND4`` parameter block contains the same parameters for ``CTFFIND4``. 
In general, the defaults work, though you may wish to increase the ``min_res`` and reduce the ``max_res`` for tomography. 
 
6.	The ``nthreads`` parameter sets parallelization for ``CTFFIND4``.
This number is used when running TOMOMAN in MATLAB, but is overridden by the SLURM parameters when submitting as a SLURM job. 
 
7.	Run ``tiltctf`` as a SLURM job. 
You can examine the results by opening the diagnostic ``.mrc`` file in the ``tiltctf/`` subfolder. 

Tomogram Reconstruction in NovaCTF
----------------

Final tomogram reconstruction for subtomogram averaging will be performed using ``novaCTF``, which allows for 3D CTF-correction during reconstruction. 
TOMOMAN will generate the appropriate scripts and output directory for running ``novaCTF`` and binning tomograms by Fourier cropping. 

1.	Open ``tomoman_novactf.param``. 
The directory parameters should already be correct.
 
2.	The parallelization parameters are only used when running within MATLAB, otherwise they are overridden with the SLURM parameters.
 
3.	Stack parameters for parameters for generating the aligned stacks prior to tomogram reconstruction. 
``ali_dim`` allows for resizing, though I recommend using the full image size. 
``erase_radius`` is for gold fiducial erasing; you should have this number from performing tilt series alignment. 
``taper_pixels`` is used to taper the edges of the rotated images when generating an aligned stack; 100 is usually sufficient. 
For this tutorial, to save on computation time, we will use an ``ali_stack_bin`` of 4. 

.. note::
    binning is performed immediately prior to tomogram reconstruction, so all other parameters are in unbinned pixels.
 
4.	The 3D CTF correction parameters set how novaCTF will perform 3D CTF correction. 
I always recommend using the dose-filtered stack and correcting CTF using phase flipping. 
For ``defocus_step``, smaller steps produce more precise results at the cost of more computation time (see :cite:`turonova_efficient_2017` for more information). 
For this tutorial, set this to ``50``. 
 
5.	Tomogram reconstruction parameters have some specifics on how to perform the reconstruction.
I generally leave skip radial filtering. The ``tomo_bin`` parameter allows you to set the final binning factors desired. 
Since we set ``ali_stack_bin`` to ``4``, the minimum allowed value here is ``4``. For this tutorial, set binnings of 4 and 8.
 
6.	The ``output_dir_prefix`` sets the name of the tomogram output directories, which will be placed within the ``root_dir``. 
For instance, bin 4 tomograms will be placed in: ``[root_dir]/[output_dir_prefix]_bin4/``. 

7.	The additional parameters include the ``recons_list``, which allows for reconstructing a subset of tomograms. 
Otherwise, all non-skipped tomograms in the tomolist will be reconstructed. 
 
8.	``Fourier3D`` is a program for Fourier cropping volumes written by Beata Turoňová. 
The ``f3d_memlimit`` parameter sets a limit to how much memory Fourier3D can use; more memory allows for faster computation times. For this tutorial, set this to ``10000``.
 
9.	NovaCTF’s approach to CTF-correction assumes that the center of mass is at the center of the tomograms; this is why we took the time to properly center the tomogram during tilt series alignment. 
If this is off, the reconstructed tomogram will contain a systematic error in all planes. 
To refine the tomogram center, novaCTF allows you to generate an offset value for recentering. 
TOMOMAN can take an input ``STOPGAP motivelist``, and use the center of mass of the particles as the refined center. 
Since we have no such motivelist now, this can be left off.
 
10.	Run ``novaCTF`` as a SLURM job. 
