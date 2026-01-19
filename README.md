# FlowMatching4Spectrograms

## Info: 

Source Dataset is a list of spectrograms of shape T x 512 where T is the clips duration (unsure of ratio to seconds). Aswell as a list of labels from 0-9 as to what digit the person is saying. 
Dataset created from https://www.kaggle.com/datasets/sripaadsrinivasan/audio-mnist. 

## Step 1: Filter the Dataset

I wanted all the samples from the dataset to have the same duration so they were either cropped, sped up, or slowed down to meet that requirement. Some samples were too long or too short so they were removed wrather than augmented to the point of major distortion. (30K -> 27K samples) 
A classifier was trained on the remaining spectrograms and the spectrograms which caused the most error were removed for being too abnormal. (27K -> 20K) 
The final specrograms were combined into a tensor of shape 20K x 24 x 256. 

## Step 2: Augmenting the Dataset 

There were 4 types of augmentations applied to the spectrograms: 
* increasing frequencies by 1 + moving forward by 1 timestep
* decreasing frequencies by 1 + moving backward by 1 timespep
* speeding up by 2 timesteps
* slowing down by 2 timesteps

These agumentations, along with the original samples, gives a dataset of size 100K x 24 x 256. 

## Step 3: Training an Autoencoder 

An autoencoder was trained on the augmented dataset to give encoded spectrograms of shape 100K x 6 x 64.

## Step 4: Training a Flow Matcher on encoded spectrograms 

A U-Net Architecture was trained on the encoded dataset to predict the flow field. 
Specifications: 
* Total of ~6M parameters 
* 768 embedding dimension for class labels (overkill, this was used for word embeddings before)
* 2 downblocks, 2 midblocks, 2 upblocks
* Trained using 50 timesteps
* 100 epochs (training converged after 20 though)

## Notebooks

DataSet Filtering: https://www.kaggle.com/code/bandrewensinger/spectrogramclassifer24x256
AutoEncoder Model Training: https://www.kaggle.com/code/bandrewensinger/spectrogramae-training-dataset-encoding
FlowMatching Model Training: https://www.kaggle.com/code/bandrewensinger/guided-flow-matching-spectrograms/edit


