Tilt-series alignment
============

IMOD Preprocessing
----------------

For this dataset, we will use TOMOMAN to do the initial phases of manual tilt series alignment using IMOD. The final tilt series alignment will be performed in IMOD’s etomo package. 

1.	Open tomoman_imod_preprocess.param. 
The directory parameters field should be correctly set.
 
2.	In the TOMOMAN parameters field, the process_stack option sets which image stack to use for tilt series alignment. 
We recommend using the dose-filtered stacks.

3.	The copytomocoms are settings for IMOD’s compytomocoms fuction. 
For this dataset, the gold fiducials have a 10 nm diameter. 
 
4.	The CCD eraser block are to run IMOD’s ccderase function; this is used to remove hot pixels. 
We recommend running this, even on direct detector data. 
We find the default IMOD parameters are sufficient and don’t include them here. 
 
5.	The coarse alignment block contains settings for IMOD’s tiltxcorr function. 
We find the default parameters to generally be sufficient, but you may wish to change the binning factors, depending on your dataset.
 
6.	Tracking choices are used for setting initial tracking method, such as automatic fiducial seeding or patch tracking. 
Parameter blocks for these approaches are below. For this practical, we will use seed and track (tracking_method = 0).
 
7.	The defaults for the autoseed and beadtrack are usually a good starting point. 
It may still be necessary to adjust the seeding manually, particularly for this older K2 dataset. 
 
8.	Run TOMOMAN in SLURM.


Manual Tilt Series Alignment in Etomo
----------------

When fiducial markers are present, such as in this dataset, we find that they are typically superior to automated alignment methods. 
Here, we will go through manual tilt series alignment in etomo. 
The parameters determined here will be used for CTF determination and tomogram reconstruction in subsequent steps. 

1.	In a terminal, load the IMOD module and go to the tilt series folder and imod/ subfolder. 
Start etomo using the .edf file generated during TOMOMAN’s IMOD preprocessing wrapper. 
There should only be one .edf file, so a wild card can be used:

::
    $ etomo *.edf
 
2.	Though Fiducial Model Generation is marked as complete, it is still worth checking the automated results. 
Click on the button on the left panel and go to the Track Beads tab. 
To open current fiducial model, click the Fix Fiducial Model button. 
 
3.	Generally, my preference is to pick fiducials that do not overlap during the tilt series and are present in every image. 
Remove fiducials or add new ones from the model as necessary. 
Save the model and run Track with Fiducial Model as Seed. 
.. note::
    There seems to be an issue with Sobel filtering when using batch processing and etomo. 
    If you need to track the seed model again, disable Sobel filtering. 
 
4.	Move on to Fine Alignment and go to the Global Variable tab. 
Since this data was collected on a Titan Krios with a well-calibrated rotation angle, I would set the solution to No rotation, Fixed magnification at 1.0, Fixed tilt angles, and no distortion solution. 
 
5.	Next, fix the center of the fiducial model. 
I do this iteratively, reducing the Threshold for residual report at each iteration from 3.0, to 2.0, to 1.0. 
.. note::
    the goal is not to minimize the residuals per se, instead you want to make sure the centering of the fiducial model is accurate. 
 
6.	After centering, you can examine the residuals for each contour in the align log. 
It can be useful to remove any contours with outlying residuals. 
 
7.	Move on to Tomogram Positioning. 
We position the tomogram by creating a boundary model using the whole tomogram. 
Since the position is unknown, make the thickness much more than what’s expected (e.g. 3000). 
Only low resolution features are required to find the specimen boundary, so use the highest binning possible. 
Create the tomogram.
 
8.	Create boundary model by setting model points on the specimen edges. 
This is typically easier to do viewing along the XZ planes. 
.. note:: 
    If you have trouble seeing the boundaries, low pass filtering the slices may help.
 
9.	 The boundary model will help IMOD determine the optimal thickness for the specimen, but this is often too thin for subtomogram averaging. 
For instance, if your particle is at the edge of the specimen, you need extra space in order to crop a volume. 
As such, I would suggest an added border thickness of an expected bin 1 subtomogram; for this dataset 100 is appropriate.
 
10.	Run compute Z shift and pitch angles. 
Before running Create Final Alignment, set the X axis tilt to 0. Accounting for the X axis tilt of the specimen causes a rotation of the missing wedge in Fourier space, which may not be accounted for in subtomogram averaging packages. 
While STOPGAP can account for applied X axis tilt, this requires extra computational cost for no benefit. 
Create final alignment. 

Final Aligned Stack
----------------

While this is IMOD’s step for creating the final aligned stack, we will do our final stack generation and tomogram reconstruction using novaCTF later. 
However, we still want to proceed on this step to have a check on our alignment and generate a gold fiducial model for erasing. 

1.	First create a stack. 
This is mainly for diagnostic purposes, so a bin 4 or bin 8 stack is sufficient. 
Reduce size with antialiasing filter. 
 
2.	View full aligned stack. 
In a properly aligned tilt series, the fiducial marker should move perfectly horizontally. 
For a basic check of the tilt series alignment, you can create a box to help guide your eye. 
In 3dmod using ctrl+b and clicking on the ZaP window, make a box where the horizontal edges touch the edge of a fiducial marker. 
If you play the stack in movie mode, you should be able to see the edge of the fiducial trace along the edge of your drawn box. 
 
3.	Move to the Erase Gold tab. 
The existing fiducial model from the fine alignment step typically doesn’t have every marker. 
We will use IMOD’s findbeads3d tool to generate a complete fiducial model. 
Findbeads3d attempts to find fiducials in a tomogram and back project them to the tilt series to generate a fiducial model.
 
4.	Align and build tomogram. Etomo will generally set a high binning factor that is appropriate for fiducial detection. 
.. note::
    Make sure the tomogram is thick enough to contain all fiducials. 
    Given the high binning factor, it might be easiest to just reconstruct a very thick tomogram. 
 
5.	Run findbeads3d. 
Check the results on the tomogram, where the centers of detected beads will be highlighted with cyan circles. 
If beads are missing, they can be added via 3dmod’s bead fixer (which should already be open). 
 
6.	Reproject model and check tilt series. 
It can sometimes be easier to notice missing beads in the tilt series. 
If you find missing beads, go back and add them on the tomogram and reproject again; this can be repeated iteratively. 
.. note::
    there are likely to be beads outside the main field of view, i.e. present only in the tilted images. 
    As they are not within the tomogram, findbeads3d will not detect them. 
    However, since their projection artifacts aren’t as strong and they project only into the edges of the tomogram, it is usually find to leave them alone.
 
7.	Erase beads and view erased stack.
.. note::
    the diameter in this section is in pixels relative to your aligned stack, not the unbinned stack. 
    Given the inconsistency in bead sizes and the additional fringing artifacts caused by the CTF, it is usually best to iterate the diameters a few times to find the best result.
     Write this number and the binning factor down; we will need it for the final reconstructions. 
