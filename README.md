# Emotional Recognition from Human Body Expression



This project aims at dectecting human emotion (positive/ negative) based on body expression

## Data Preparation
Raw data: 48 videos with different movements from different people, labeled as positove/ negative
### Video Processing
With OpenPose, each video is transformed into a series of 25 Keypoint coordinates with 1/5 second interval, saved in a JSON file



### Data Interpolation
Situation 1: there are 2 data points before and after the NAs, and not too many NAs in between
  then deal with linear interpolation
Situation 2: only a few NAs at the begining rows
  then backward filling
Situation 3: only a few NAs at the ending rows
  then forward filling
### Data Transformation
Distance: the distance between 2 Keypoints using Euclidean distance
Angle: the angle between 2 distances using arccosine formula

![image](https://user-images.githubusercontent.com/74312026/119430213-5df48680-bcde-11eb-8fb1-15f11f0b7a5b.png)
(4 Keypoints are removed: 17, 18, 20, 21, 23, 24)

In total, we get 40 features including distances and angles. And all videos are split into 39 training videos and 5 testing videos
### Data Augmentation
In each video JSON file, there are hundreds of timesteps. Since the model is aimed at conducting real-time recognition, all timestpes in each video JSON file are converted into some 10-timestep timeseries data. ??????no duplicates or with sliding window

## Modeling
The model consists of 3 parts: 19 LSTM subnetworks, 19 Temporal attention models, 1 Bodily attention model
19 represent 19 body parts
### 19 LSTM subnetworks






https://github.com/CMU-Perceptual-Computing-Lab/openpose
