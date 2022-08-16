Bin4 Subtomogram Averaging with STOPGAP
=============

The main goal here is to prepare the motivelist for bin2 processing. This includes rescaling the motivelist, applying the shifts to the extraction positions, and splitting halfsets. 

1.	In MATLAB, we will re-number the particles and assign odd/even halfsets:
>> sg_motl_assign_halfsets('allmotl_dclean_sclean_2.star',
   'allmotl_dclean_sclean_halfset_2.star','oddeven',1);
NOTE: Since we have renumbered the subtomograms, this new motivelist will NOT work with the current averaging folder.
 
2.	Rescale the motivelist. This will rescale the extraction positions and apply shifts. 
>> sg_motl_rescale('allmotl_dclean_sclean_halfset_2.star','allmotl_bin4_1.star',2,1);
 
3.	Make a new subtomogram averaging folder. The wedgelist keeps all information in unbinned pixels, so it can be used for any binning. The paths in the STOPGAP tomolist need to be updated; this can be done by making a one or simply using a text editor. Copy the necessary bash scripts.
 
4.	Extract subtomograms. Set the boxsize to 64 and update the pixelsize. 


Bin4 Angular Refinement
-----------------

Here, we will go over how to set up parameters for angular refinement.

1.	Before we can generate our first average, we need to provide an alignment mask. As with our bin8 processing, we can generate a simple sphere for this step. E.g.:
>> sphere = sg_sphere(64,26,3);
>> sg_mrcwrite('masks/sphere.mrc',sphere);
 
2.	Generate average using bin4 motivelist. 
 
3.	Generate a new alignment mask. A cylinder should be sufficient again, but this time make one that only includes the outer structured layer. For my average, this works:
>> cyl = sg_cylinder(64,26,22,3,[33,33,32]);
>> sg_mrcwrite('masks/cyl_mask.mrc',cyl);

4.	Generate a new CC mask. Since we’ve already determined the true particle positions at bin8, the goal is no longer to generate a CC mask that allows our randomly seeded particles to find the nearest true particle. Instead, we want to make a CC mask that allows for proper sampling but not too large that results in getting trapped in false local minima. Arguably, a sphere with a 2 pixel radius should be sufficient to account for the bin8 precision, but a 4 pixel radius should still be safe. 
 
5.	Set alignment parameters. We can keep the same low-pass filter resolution for the first alignment run. Since we have unbinned by a factor of 2 and increased the boxsize by a factor of 2, the lp_rad should still be the same. For angular alignment, the angincr of 2deg we used earlier should still be sufficient (in practice, 2 deg will get you well past 10 Å). Since our box is still relatively small, we can leave the angiter at 3 without too much computational expense. In our last run, the phi_angincr and phi_angiter were set to sample a full 60 deg range. At this point, our angular error in phi is likely somewhere around 4 deg. As such, setting phi_angincr and phi_angiter to 2 and 3 should provide enough sampling. 
 
6.	Parse subtomo parameters and run alignment. Looking at the output FSC, the average is nearly at sub-nanometer resolution, though we are limited in visualizing this due to pixelsize. Given that we have only used 60 Å information in our alignment, this is clear example that the resolution of high-resolution features is driven by the alignment of low-resolution data. 
 
7.	As noted above, 2 deg angular increments should be sufficient, so our main parameter to change is our low-pass filter. Given that our resolution is so high, we can safely set our low pass radius to ~20 Å (17 Fourier pixels). Since we have limited computational power during this practical, we can play with some methods for cutting down computation time. Set the angular iterations to 2 and the search mode to “shc”. 

“shc” stands for Stochastic Hill Climbing (SHC). In standard hill climbing, the goal is to sample all possible orientations (in our search range) and take the highest scoring one; i.e. moving up the hill as quickly as possible. SHC instead randomizes the list of search angles, scores the prior best angle, and accepts the first better orientation; you are still moving up the hill, but potentially not as quickly as possible. 

Even though alignments are somewhat suboptimal, it results in a better reference more quickly, so more iterations can be done in the same amount of time. Low to medium resolution information, i.e. the information you are using to align, is typically still well-resolved, so further iterations will still improve the overall alignment of the dataset. 

NOTE: SHC is only really useful when refining angles and NOT during de novo reference generation or finding true particle positions from oversampled starting positions. 
 
8.	At this point, the rest of the refinement towards the dataset’s information limit is largely the same process. However, there are some peculiarities with interpreting the structure of continuous surface, where the structure runs off the box edges. We will go over some of basic post-processing steps in the next section.


Post-Processing
-----------------

STOPGAP’s FSC calculations during subtomogram averaging are required for figure-of-merit weighting references prior to alignment. As such, it is essential that they are calculated on the full alignment particle. This is different than when you are interpreting your structures, where only the central subunits are important. Here, will generate an FSC mask to focus on the central hexamer, calculate FSC, and generate a sharpened reference. 

1.	Generate a cylindrical mask that focuses on the central subunit. This will likely be similar to your alignment mask, but smaller in radius. One I made was:
>> fsc_mask = sg_cylinder(64,12,22,3,[33,33,32]);
>> sg_mrcwrite('masks/fsc_mask.mrc',fsc_mask);
 
2.	In MATLAB, open sg_calculate_FSC. Adjust input files and fill ref_avg_name; a bfactor of -100 is a reasonable starting number. Run the script.
 
3.	You should find that the FSC plot is significantly better than what STOGAP outputs. The output reference should also be less noisy and sharper. NOTE: FSC estimations can be more accurate with tighter “body” masks, such as those generated using RELION. 
