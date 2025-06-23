This repository contains all scripts used for the paper "Early White Matter Microstructure Alterations in Infants with Down Syndrome" for reproduiblity purposes


Overview: This repository is organized into 4 main components 

Scan conversions: DICOMs are converted to nrrd per DWIConvert_4.6.0. Tbe script used is not necesary for reporducibility purposes and thus is not included - you may use whatever tool you like to complete this conversion. 

Preliminary QC: AFter obtaining nrdd files, a preliminary quality control is done for each subject on their 101_AP, 101_PA using 3D Slicer 5.6.2 and 101_PA_B0 using DTIPrep 1.2.8. During this preliminary quality control, number of gradients, artifacts and motion are checked. Subjects are excluded if they have below a 70% threshold for number of gradients or if there is a severe reference image artifact/motion in the cerebrum. Subjects that pass this step have nrrd file links created. 

Two scripts are run after this is completed: 
1. 2_dmriprep.script => conducts automated QC, eddy current, susceptiblity correction and generates QC tractogrpahy. It creates dmriprep_0.5.7 folders for each passing subject.
   a. Run_dmriprep_convert_wrapper.script* 
   b. Run_dmriprep_tract_wrapper.script*
   c. Run_dmriprep_wrapper.script*
2. 3_DWIMask_NIRAL.script => creates low-shell DWI (for tensor computation) and automated DWI masks (applying them as well). The end result is a subject specific folder labelled Brainmask_0.5.7.

Automated brainmasks are reviewed and if needed, manually corrected using ITK-SNAP 4.0.2

Final QC: Final quality control is done for each subject after their dmriprep_0.5.7 folder has been created. Brain tracts are reviewed using Slicer 5.6.2 and bad gradients from the complete set of 101 gradients are manually removed using dmriprep. Subjects at this stage fail either because the brain tracts were interrupted or if too many gradients were manually removed (70% threshold).

*** Note that at this stage is also when we make notes about the extent of the temporal arc artifact as decirbed in our paper. If the temporal arc artifact is impacting the frontal lobe of the subject, then that subject is excluded during this step. 

After Final QC is completed, two scripts are run: 
1. 4_MoreQCstats.script => computes motion stat csv
2. 5_NODDI.script => computes NODDI maps (creation of AMICO folder for subjects)
   a. Run_AMICO_wrapper.script*

Mapping, FiberProfiles and FiberAverages
1. 2_MapOtherDataInAtlas.script => Mapping all data into DTI atlas with tracts
2. 3_MapNODDItoAtlas.script => Mapping all NODDI data into atlas
3. dtitractstat.script => Generates fiber profiles for DTI maps
4. dtitractstat_NODDI.script => Generates fiber profiles for NODDI maps
5. GatherCSV_NODDI.script => Generates CSV for NODDI maps  
   a. RunGather.script called by GatherCSV_NODDI.script 
7.   GatherCSV.script => Generates CSV for DTI profiles 
     a. RunGather.script called by GatherCSV.script 
9.   RunCompute_oazrak.script => Computing tract averages for subjects 
