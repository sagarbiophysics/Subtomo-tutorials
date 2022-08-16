Particle picking
============

In this section, we will pick spheres in UCSF Chimera using `Kun Qu’s Pick Particle plugin <https://www.biochem.mpg.de/7940000/Pick-Particle>`_. 
We will then update these metadata on the tomolist and generate a STOPGAP motivelist of these particles. 
We will then visualize the particle positions in UCSF chimera using `Kun Qu’s Place Object plugin <https://www.biochem.mpg.de/7939908/Place-Object>`_. 

Picking Spheres
----------------


In chimera, we will pick centers using Volume Tracer and set radii for each sphere using the Pick Particle tool. 
The Pick Particle plugin is not standard in Chimera, but we have set it up in the chimera module for this course.

1. Open a terminal and load the chimera module. 
Start chimera. Open a bin 8 tomogram. 
Chimera may open the tomogram as an isosurface volume; if so visualize it in planes and set the appropriate levels. 
 
2. In the Volume Viewer panel, go to features, and show the Coordinate panel. 
Set the Origin index to 0 and the voxel size to 1. 
You may need to recenter the view; the focus command will help here.
 
3. If you want more contrast to better visualize the HIV particles, you can apply a gaussian filter (Volume Viewer > Tools > Volume Filter). 
A gaussian with of 1 should be sufficient, but remember to uncheck the “Displayed subregion only” option. 
 
4. Before picking HIV particles, it may be useful to shift the camera to use orthographic projections (Side view > Camera > Projection > Orthographic).
 
5. In the Volume Viewer, open the Volume Tracer tool. 
I would suggest setting the marker radius to 20, which makes a sphere smaller than the HIV particles, but large enough to assess centering. 
Move through the place and place a marker at each HIV particle. 
I recommend taking only the complete particles.
 
6. When you have finished marking your particles, save the marker set into the tilt series folder in as metadata/sphere/sphere.cmm. 
This naming convention is important for parsing the metadata files into the tomolist later. 
 
7. In the main window, open the Pick Particles tool (Tools > Utilities > Pick Particle). Load the marker file and press Display. 
A series of sliders will a appear for each marker. Adjust them until the radius for each as needed. 
Since the HIV particles are not perfect spheres, adjust the radius to a point with the best compromise. 
Also, there are several concentric layers to each HIV particle; set the radius for each sphere so that the edge is on similar layers. 
For these particles, I suggest setting the radius to between the two outermost layers. 
 
8. When all radii are set, press the save button. 
For now, you can click reset to remove the spherical wireframes and close the Pick Particles tool. 
Leave chimera open, but we can go back to MATLAB. 


Generating Motivelists
----------------

After picking our spheres, we will first append this data to the tomolist, and use functions in the STOPGAP toolbox to generate a motivelist.

1. We will add the picked spheres to the tomolist using the ``tm_metadata_add_new`` function. 
The input parameters are ``root_dir``, ``tomolist_name``, and ``type``. 
This function loads a tomolist, goes through each tilt series directory and scans the metadata/ subfolder for a subfolder named type, and stores the names of all files in that folder. 
These types can then be used by other functions that parse the tomolist. In this case, type is ``‘sphere’``. 
 
2. To generate a motivelist, we will use the sg_motl_batch_sphere function. 
The parameters for this function are:
``tomolist_name`` – Name of tomolist
``output_name`` – Name of output motivelist (file extension should be .star)
``metadata_type`` – Type of metadata; in this case ‘sphere’
``binning`` – Binning of input files
``p_dist`` – Distance between particles; set to half the true distance (e.g. 3.5) for oversampling
``rand_phi`` – Randomize starting phi angles; set to 1
``padding`` – Removes particles around the edge of tomograms using padding distance; set to 16
``subset_list`` – Optional file listing  a subset of tomograms to process
As an example:

::

     sg_motl_batch_sphere(‘tomolist.mat’, ’allmotl_1.star’, ’sphere’,  8 ,3.5, 1, 16, []);


3. The Place Objects tool in chimera only opens AV3-format ``.em`` files, so we will need to convert our STOPGAP motivelist using the ``sg_motl_stopgap_to_av3`` function. 
 
4. In chimera, from the main window open the Place Object function ``(Tools > Utilities > Place Object)``. 
Open the ``.em`` file and apply. This will display the particle positions as spheres, colored by their class number. 
 
5. To visualize particles with angular information, you can change the Object style using the dropdown. 
Hexagons with voxel-size 0.1 are reasonable here. 
Notice that the particle positions are evenly spaced with their planes along the surface of the spheres but their in-plane angles (phi) randomized. 
