

Welcome to the .readme for using the Magnetic Gradient Scale Length script! For the first time using, make sure you do the following:

1) Have the following installed:
	a) REGCOIL ; commit hash: 4813f74
	b) simsopt ; commit hash: dae548e (to read virtual casing files)
	d) NetCDF4 ; version: 4.8.0 (to read vmec aka wout files)

while NOT needed to run these scripts, the following may be needed to create new configurations
	1) STELLOPT; commit hash: 5896abd
		a) Compile xborm from STELLOPT (xbnorm is used in REGCOIL for finite beta configurations)
		b) Complie VMEC2000 from STELLOPT

2) update .bashrc to set absolute path to the directory the regcoil application is located in.

3) Make sure you put all the necessary vmec input files in /configs/ , the woutfiles in /configswout/, and for finite beta configs, put the bnorm files in /xbnorm/, and the virtual casing files in /vcasing/. All configurations used in the Magnetic Gradient Scale Length paper are in this zanotoFile (with the parameters used)

4) Make sure you tweak any paramters in the USER PARAMETER Section in ./MagneticGradient.py and infrastructure.py. Specfically, make sure TargetMaxK and TargetRMSB are the same numerical string in both cases.

5) Run ./infrastructure.py to set up the subfolder infrastructure (the parent directory will be named based on the target RMSB and Kmax)

6) Run ./MagneticGradient.py num 
(Replace num with an integer). This will first pull up an ordered array of all configurations that have associated folders (created by ./infrastructure.py). For example "./MagneticGradient.py 0" will run the script using the 0th configuration in the array. With the attatched set of configurations, this will be configuration wout file "wout_1020_1080_0930_0843_+0000_+0000_01_166_fixedb_aScaling.nc", which is the reactor scale version of the "high narrow mirror" variation of W7-X.


########################################################################################################################################################################

When ./MagneticGradient.py is run, the following occurs:
	a) For the chosen configuration, L_gradB and all magnetic gradient alternatives are found.
	b) Using L_gradB to help determine an initial guess, L_regcoil is found in an optimizer (This creates a lot of vmec wout files, since one is created for each iteration!)
	c) Key data from a) and b) are stored in a csv file in the created parent directory. We have put the dataset from our paper in here, named "/finalData_RMSB_0.01_targetMaxK_17.16e6_REGRES.csv"
	

The following files are also included:

1) regcoil_in.data, which contains parameters to pass on to regcoil. Make sure to keep this in teh same directory as ./MagneticGradient.py and ./infrasturcture.py, since they copy and modify regcoil_in for each configuraiton. 
2) job.mutter, which is a bash script for the Perlmutter NERSC supercomputer that runs ./MagneticGradient.py in parallel to quickly find L_gradB for all configurations. 
3) scaleLengths.py, which will find just L_gradB and alternate scale lengths of ALL configurations sequentially and record them in scaleLengths.txt . This is much faster, and requires no additional arguments, but does not calculate L_regcoil, the actual plasma-coil separation.

#######################################################################################################################################################################
