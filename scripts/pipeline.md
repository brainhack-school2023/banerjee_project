#This bash script contains all the commands that are needed to reproduce the results mentioned in this abstract
#Please send an email at banerjee.rohan98@gmail for any queries


#Step 1
#A sample initial data has been provided in this repository by the name 'data_leipzig_rest'

'''
The below script converts data from fMRI BIDS format (received from different sites) to ivadomed compatible BIDS format
The fMRI BIDS format looks like the following:

data_{site_name}_{task}
├── derivatives
│   ├── labels
│   │   ├── sub-01
│   │   │   ├── func
│   │   │   │   ├── sub-{number}_{task_name}-spinalcord_mask.nii.gz
│   ├── moco
│   │   ├── sub-01
│   │   │   ├── func
│   │   │   │   ├── sub-{number}_{task_name}-mocomean_bold.nii.gz

The ivadomed compatible BIDS format looks like the following:

data_{site_name}_{task}_BIDS
├── derivatives
│   ├── labels
│   │   ├── sub-{number}
│   │   │   ├── func
│   │   │   │   ├── sub-{number}_{name}_seg-manual.nii.gz
│   │   │   │   ├── sub-{number}_{name}_seg-manual.json

├── sub-01
│   ├── func
│   │   ├── sub-{number}_{name}_bold.nii.gz
│   │   ├── sub-{number}_{name}_bold.json
├── dataset_description.json
├── participants.tsv
├── participants.json
├── README

'''

python BIDSify_fMRI_data.py --input_path "./data_leipzig_rest/derivatives/moco" --label_path "./data_leipzig_rest/data_leipzig_rest/derivatives/label" --output_path "./fmri_sc_seg/data_leipzig_rest_bids"


#Step 2
#After this step, the BIDSified data, the data has to be transformed into the Medical Segmentation Decathlon (MSD) format. You can read more about the format at -- https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/dataset_format.md#how-to-use-decathlon-datasets.
#The MSD format looks like the following:

'''
data_{site_name}_{task}_MSD
├── imagesTr
│   ├── sub-{number}_{name}_T1w.nii.gz
│   ├── sub-{number}_{name}_T2w.nii.gz
│   ├── sub-{number}_{name}_bold.nii.gz
├── labelsTr
|   ├── sub-{number}_{name}_seg-manual.nii.gz
├── imagesTs
│   ├── sub-{number}_{name}_T1w.nii.gz
│   ├── sub-{number}_{name}_T2w.nii.gz
│   ├── sub-{number}_{name}_bold.nii.gz
├── labelsTs
|   ├── sub-{number}_{name}_seg-manual.nii.gz
├── dataset.json
'''

python convert_bids_to_nnunet.py --path-data ./fmri_sc_seg/data_leipzig_rest_bids --path-out /path/to/output/directory --taskname fmri_sc_seg --tasknumber 502 --split-dict /path/to/ivadomed-split/dictionary


#Step 3
#Once the dataset is converted to MSD format, the dataset can be used to train the nnUNet. The nnUNet training is generally done using the 5-fold cross validation and it default runs for 1000 epochs. Please read the nnUNet documentation here -- https://github.com/MIC-DKFZ/nnUNet/tree/master/documentation.
#First, we set the paths:
export nnUNet_raw="./nnUNet_raw"
export nnUNet_preprocessed="./nnUNet_preprocessed"
export nnUNet_results="./nnUNet_results"

#The command below is used to preprocess the data. The nnUNet decides the preprocessing steps on it's own based on the dataset. The preprocessing steps are stored in the folder nnUNet_preprocessed.
nnUNet_plan_and_preprocess -d 502 --verify_dataset_integrity

#Step 4
#The command below is used to train the model. The model is trained using the 5-fold cross validation. The trained models are stored in the folder nnUNet_trained_models. You can train the 5 different folds on 5 different GPUs in parallel to save timne.
CUDA_VISIBLE_DEVICES=xxx nnUNetv2_train 502 2d fold_number --npz

#Step 5
#Once training of all the 5 folds is completed, run the following to find the best configuration for the model among the five folds.
nnUNetv2_find_best_configuration Dataset502_xxx -c 2d

#Step 6
#For running inference, run the following command.
nUNetv2_predict -d Dataset501_xxx -i INPUT_FOLDER -o OUTPUT_FOLDER -f  0 1 2 3 4 -tr nnUNetTrainer -c 2d -p nnUNetPlans

#Step 7
#For postprocessing of the predictions, run the following command.
nnUNetv2_apply_postprocessing -i path_to_imagesTs -o path_to_postprocessing_path -pp_pkl_file path_to_postprocessing_pkl_file -np 8 -plans_json ./nnUNetTrainer__nnUNetPlans__2d/crossval_results_folds_0_1_2_3_4/plans.json










