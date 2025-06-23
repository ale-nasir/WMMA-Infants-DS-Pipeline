This repository contains all scripts used for the paper "Early White Matter Microstructure Alterations in Infants with Down Syndrome", for reproducibility purposes. 

Overview: 
This repository is organized into four main components labelled: Preliminary QC, Final QC, Atlas Mapping and Fiber Processing 

Before diving into the four components, note that DICOMs are first converted to .nrrd format using DWIConvert_4.6.0. 
The specific script used is not necessary for reproducibility and is therefore not included — you may use any tool of your choice to complete this conversion. 



PRELIM_QC: After obtaining .nrrd files, a preliminary quality control (QC) step is performed for each subject 
- 101_AP and 101_PA are checked using 3D Slicer 5.6.2 
- 101_PA_B0 is checked using DTIPrep 1.2.8 

During this step, the number of gradients, artifacts, and motion are assessed. Subjects are excluded if: 
- Fewer than 70% of gradients remain 
- There is a severe reference image artifact or significant motion in the cerebrum 

Two scripts are run after preliminary QC:  

2_dmriprep.script -> Performs automated QC, eddy current correction, susceptibility correction, and generates QC tractography. It creates a dmriprep_0.5.7 folder per passing subject. 
This script calls the following as well:  
   - Run_dmriprep_convert_wrapper.script 
   - Run_dmriprep_tract_wrapper.script 
   - Run_dmriprep_wrapper.script 

3_DWIMask_NIRAL.script  -> Creates low-shell DWI images (for tensor computation) and generates automated DWI brain masks. Final output is a subject-specific Brainmask_0.5.7 folder. 

Automated brain masks are then reviewed and manually corrected using ITK-SNAP 4.0.2. 



FINAL_QC: Final QC is conducted after the dmriprep_0.5.7 folder is generated for each subject.  
- Brain tracts are reviewed using 3D Slicer 5.6.2 
- Poor-quality gradients are manually removed from the full set of 101 gradients using dmriprep 
- Presence and extent of the arc artifact in the temporal and frontal lobes 

Subjects are excluded at this stage if: 
- Brain tracts show major interruptions 
- More than 30% of gradients must be removed 
- Frontal lobe is impacted by the temporal arc artifact 

After Final QC, two scripts are executed: 

4_MoreQCstats.script -> Computes motion stat csv 

5_NODDI.script -> Computes NODDI maps (creates AMICO folders for subjects) 
   - Calls Run_AMICO_wrapper.script



ATLAS_MAPPING : Two scripts are executed

2_MapOtherDataInAtlas.script -> Maps all data into DTI atlas with tracts 
   
3_MapNODDItoAtlas.script -> Maps all NODDI data into atlas 



FIBER_PROCESSING : Five scripts are executed

dtitractstat.script -> Generates fiber profiles for DTI maps 

dtitractstat_NODDI.script -> Generates fiber profiles for NODDI maps 

GatherCSV_NODDI.script -> Generates CSV for NODDI maps   
   - Calls RunGather.script 
   
GatherCSV.script -> Generates CSV for DTI profiles  
   - Calls RunGather.script 
     
RunCompute_oazrak.script -> Computes tract averages for subjects 
