# Rohan Banerjee

## About me

Hello everyone, I am Rohan Banerjee, a first year Master's student at the NeuroPoly Lab, Polytechnique Montreal working under the guidance of Dr. Julien Cohen-Adad and Dr. Benjamin De Leener. My work revolves around developing automatic segmentation methods for various anantomies like spinal cord (SC), cerebrospinal fluid (CSF) etc. in SC MRI images especially fMRI and developing a SC template generation pipleline.

<a 
href="https://github.com/rohanbanerjee">
   <img src="https://avatars.githubusercontent.com/u/25586344?v=4?s=100" width="100px;" alt=""/>
   <br /><sub><b>Rohan Banerjee</b></sub>
</a>

## Project Summary

Functional magnetic resonance imaging (fMRI) of the spinal cord is relevant for studying sensation, movement, and autonomic function. Data processing involves segmenting the spinal cord on axial gradient-echo echo-planar imaging (EPI) data. However, this task can be arduous due to the inherent limitations of the data, including low spatial resolution, susceptibility artifacts, ghosting, and motion-related distortions. For example, 

<p align="center">
  <img src="images/low_contrast.png" alt=""/>
</p>

These limitations make it challenging to accurately delineate the boundaries of the spinal cord. Consequently, this particular segmentation task demands considerable manual effort, consumes a significant amount of time, and is prone to inconsistencies between different observers and even within a single observer. To address these challenges, I propose an automatic approach that leverages deep learning techniques for the automatic segmentation of spinal cords in fMRI gradient echo EPI data. The proposed model is trained on data collected from multiple sites, enabling it to learn and generalize from a diverse range of inputs. By incorporating this broader range of data, we aim to enhance the robustness and adaptability of our method. 


## Objectives

Develop an automated spinal cord segmentation method which has a special focus for fMRI data. This objective consists of the following sub-objectives:
* SO1: Collect and process fMRI data from different sites across the world.
* SO2: Design an automated segmentation method which uses this large-scale data and is generalizable for out-of-distribution data.
* SO3: Validate the segmentation results by using it for other methods like motion correction, spinal cord normalisation.
* SO4: Package and integrate this method as a part of the spinal cord toolbox (SCT) for the research community to use it out-of-the-box.



## Data
The dataset contains fMRI EPI spinal cord images along with manual segmentation in the BIDS data format. The images look like the below:

<p align="center">
  <img src="images/dataset.png" alt=""/>
</p>

The data is being scanned from multiple sites across the world. For this project, I will be using the data from:
* Max Plank Institute, Leipzig, Germany (a subset of the dataset can be found here --> [Dataset subset](https://github.com/sct-pipeline/fmri-segmentation/tree/main/data_leipzig_rest))
* École Polytechnique Fédérale de Lausanne, Geneva, Switzerland

This dataset will be soon open-sourced to Openneuro for researchers to use.


## Tools




## Methods overview

## Deliverables

## Results 

## Reproducibility

## Conclusion

## Acknowledgements

## References