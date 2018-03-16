# pet_converting
This repo is a set of scripts dedicated to converting PET images into DICOM files. In a research setting, it is
common to convert away from DICOM and into formats such as Nifti and MGH. In clinical settings, however,
many of the programs for viewing images operate on DICOM formatted files so there are many instances where
we want to convert into DICOM format.

## conv_ecat2nii.py
This script was built off of Cindee Madison's (ecat2nii.py script)[https://gist.github.com/cindeem/7347843]. 
It looks for a text file in the current working directory called "pet_studies.csv", with the following format:

'''
ECATpath,PatientID,ScanDate
SUB1919/PET_vol_3ffa23a06_dse7.v,SUB1949,20150413
SUB4266/PET_vol_4212asd6f_daa5.v,SUB4360,20171203
SUB5179/PET_vol_4378saca0_d513.v,SUB5869,20160215
SUB6263/PET_vol_44awwe3c2_ee19.v,SUB6869,20150123
'''

The script reads ECAT volumes listed in the text-file and converts them into Nifti volumes - one nifti file per
frame. These output Nifti volumes are saved into a directory called './nvols/'. Further, a new text-file
called "pet_studies_nifti.csv" is generated. It's a copy of the "pet_studies.csv" file, but with a new column that
has the output directory associated with the ECAT to Nifti conversion process.


## conv_nii2dcm.py
This script uses (Python Mango Scripting)[http://ric.uthscsa.edu/mango/scripttutorial.html] to convert each
Nifti volume into a single 3D DICOM file. The output files are saved into './dvols/'. These DICOM files
can be read into and manipulated with Osirix or other DICOM viewers.

The script reads the "pet_studies_nifti.csv" from the conv_ecat2nii procedure above. Another weird requirement that
I'd like to apologize for is that the script requires the existence of a file called "test_out.nii.gz". Any Nifti
volume should work. For some odd reason, the Mango scripting requires inputting a file name for a volume and also requires
that the file exists.

## conv_dcm2dfiles.py
This script breaks down the 3D DICOM volumes into individual slices (i.e. one DICOM file per slice). The files are
outputted to './dfile'. I still have to test it more extensively. Getting the orientation correct is particularly
tricky business, but I've gotten it to work on the first couple scans.

## Requirements
Python 2.7, Nibabel, Pandas, Pydicom, and the Python Mango Scripting API

## Warranty
None
