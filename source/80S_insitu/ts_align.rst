Tilt-series Alignment
===============

Fiducial-less Alignment using AreTomo with TOMOMAN
-----------------

As this turorial dataset consists of the tilt-series acquired on cryo-FIB milled lamellae, we can't use fiducial based alignment as in the case of HIV practical.
However, multiple fiducial-less methods have been developed. 

Notably: 
- Patch tracking in IMOD
- fiducial-less alignment using ALIGNATOR
- fiducial-less alignment using AreTomo

For this practical, we will use TOMOMAN functionality for fiducial-less alignment using AreTomo.
I would strongly recommend to familiarize oneself with the AreTomo Manual. 

1. Open ``tomoman_aretomo.param`` file. 
The directory parameters should already be correct.

2. ``out_dir`` field points to a directory inside the ``root_dir`` where output tomogram reconstructions from AreTomo are saved. 
In this case, we will be using the default value of 8 for ``OutBin`` parameter under ``AreTomo Parameters`` block. 
Hence, we will use the default value for ``out_dir`` as ``bin8_aretomo/``. 
In case one choses to use other binning value for ``OutBin``, make sure that ``out_dir`` reflects the set binning value. 

3. ``process_stack`` allows one to chose either dose-filtered or unfiltered stack for tilt-series alignment. 
I would recommend to use ``dose-filtered`` stack in most cases. 

.. note::
    When using ``dose-filtered`` stack, make sure to set ``DarkTol`` parameter under ``AreTomo Parameters`` block to ``0.001`` (the default value.).
    
4. The ``AreTomo Parameters`` block sets values for ``AreTomo`` input arguments. 
Although we discuss these parameters in brief, I would emphasize on reading through the AreTomo manual. 

5. ``InBin`` sets binning of the input tilt-series. 
For example, in case of the default value ``4``; TOMOMAN will first bin the tilt-series by the factor of 4 and use that as an input for AreTomo. 
Leave this to default. 

6. ``AlignZ`` and ``VolZ`` set the expected thickness of the sample and the thickness of reconstructed volume. 
Both these values are set in terms of unbinned pixels, irrespective of value set for the ``InBin``. 
In out experience, ``AlignZ`` value of ``800`` unbinned pixels works best for this dataset. 
This will depend on the expected thickness of the sample and also on the magnification at which the data was aquired. 
``VolZ`` parameter sets the thickness of the reconstructed volume. 
In order to space and as the samples in this example dataset are not thicker that ``2048`` unbinned pixels, we will set ``VolZ`` to ``2048``.

.. note::
    At the moment, ``VolZ`` sets the thickness of the tomogram in all downstream reconstructions using ``novaCTF``. 
    
7. As discussed earlier we will use ``OutBin`` of ``8``. 

8. This dataset consists of tilt-series acquired on FIB-milled lamellae. 
Most often such lamellae are milled at an angle to the grid/support material. 
This results in a ``pre-tilt`` of the sample, and the data acquition for dose-symmetric scheme is often started at said ``pre-tilt``. 
``AreTomo`` can solve for this ``pre-tilt`` and correct for it, when ``TiltCor`` parameter os set to ``1``.

9. We will leave ``FlipVol`` and ``FlipInt`` values to default.

10. AreTomo can reconstruct output tomographic volumes using either ``SART`` or ``WBP``. 
In this case we will set ``SART`` to ``none`` and ``WBP`` to ``1`` in order to get output volumes using ``WBP``. 

11. In addition to the global tilt-series alignment, AreTomo can also use either a regular grid (patches) or user defined Regions of Interest (ROIs) to solve for local deformations. 
We will not use this functionality for the practical as the local tranforms are not compatible with downstream subtomogram averaging workflows.

12. ``OutIMOD`` and ``OutXF``, when set to ``1``, AreTomo will write global transformations in IMOD formated ``.xf`` file. 

13. Leave ``DarkTol`` to the default value of ``0.001``. 
When this value is set higher, AreTomo will automatically exclude darker tilt images. 

14. When ``ForceAlign`` is set to ``1``, TOMOMAN will also rerun the AreTomo task for already processed tomograms. 

15. Save Changes to the ``tomoman_aretomo.param``. 
Submit this TOMOMAN job to SLURM. 
First, make sure the bash file has the correct ``paramfilename``. 
Then, in a terminal, run the bash script.

16. Once the job is finished, you can open the reconstructed tomogram in ``bin8_aretomo/`` folder using ``3dmod``.
