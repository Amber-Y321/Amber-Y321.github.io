# Emotional Recognition from Human Body Expression



This project aims at dectecting human emotion (positive/ negative) based on body expression

## Data Preparation
Raw data: 48 videos with different movements from different people, labeled as positove/ negative
#### Video Processing
With OpenPose, each video is transformed into a series of 25 Keypoint coordinates with 1/5 second interval, saved in a JSON file

![image](https://user-images.githubusercontent.com/74312026/119430213-5df48680-bcde-11eb-8fb1-15f11f0b7a5b.png)
(4 Keypoints are removed: 17, 18, 20, 21, 23, 24)

Therefore, 19 keypoints are kept
#### Data Interpolation
Situation 1: there are 2 data points before and after the NAs, and not too many NAs in between
  then deal with linear interpolation

Situation 2: only a few NAs at the begining rows
  then backward filling

Situation 3: only a few NAs at the ending rows
  then forward filling
#### Data Transformation
Distance: the distance between 2 Keypoints using Euclidean distance
Angle: the angle between 2 distances using arccosine formula
In total, we get 40 features including distances and angles. And all videos are split into 39 training videos and 5 testing videos
#### Data Augmentation
In each video JSON file, there are hundreds of timesteps. Since the model is aimed at conducting real-time recognition, all timestpes in each video JSON file are converted into some 10-timestep timeseries data to make sure that end-user could get real-time customer experience

Method: Run a sliding window, length = 10 timesteps, overlapping ratio = 60%

## Modeling
The model consists of 3 parts: 19 LSTM subnetworks, 19 Temporal attention models, 1 Bodily attention model
19 represent 19 body parts, but there are some variables (angles and distances) used to describe different body part
#### 19 LSTM subnetworks
For each LSTM subnetwork related to each body part:

<img width="400" alt="image" src="https://user-images.githubusercontent.com/74312026/154887844-d4352276-bc8d-411d-9962-50d0d8154303.png">

#### 19 temporal attention models
Each temporal attention model aims to extract key timestemps:

<img width="521" alt="image" src="https://user-images.githubusercontent.com/74312026/154891764-845ee68d-cd71-41f8-986f-c5118bc02707.png">

#### bodily attention models
The bodily temporal attention model aims to extract key body parts:

<img width="518" alt="image" src="https://user-images.githubusercontent.com/74312026/154892316-6b71d19c-ba1c-4662-9310-e257c2767bd4.png">

#### The whole model flowchart

<img width="631" alt="image" src="https://user-images.githubusercontent.com/74312026/154892419-b934b9db-a43e-4e23-ac04-437fbde9337d.png">

## Tunning Process
Adam optimizer: more intellignet since it could remember and store an exponentially decaying average of the past gradients and bring in a smoothening effect
learning rate = 0.0001, epoch number = 70, batch size = 16

## Results and Interpretation
Accuracy = 0.81
Precision = 0.83
Recall = 0.97
F1 score = 0.89

For predicted negative experience, we can consider them as negative with high probability; but the model has lower ability to predict positive experience,so more positive data is needed to improve the model to extract positive experience information. 

## Reference
https://github.com/CMU-Perceptual-Computing-Lab/openpose
