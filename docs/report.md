# Table of Contents
* Abstract
* [Introduction](#1-introduction)
* [Related Work](#2-related-work)
* [Technical Approach](#3-technical-approach)
* [Evaluation and Results](#4-evaluation-and-results)
* [Discussion and Conclusions](#5-discussion-and-conclusions)
* [References](#6-references)

# Abstract

This project implements a human activity recognition system in order to classify data into five different classes namely -boxing , jacks, jumping, squats and walking. In the implementation, point cloud data is obtained using the mmwave radar. This is in turn voxelised and classified using a CNN-LSTM model to obtain the output. Although several models exist for this classification, our goal is to obtain live data and deploy the optimised and most efficient model on a Jetson nano platform to provide an output with the least latency. Post setting up the entire pipeline, we could successfully obtain live data from the radar and were able to interface it with the Jetson nano module provided to perform voxelisation and classification individually to each set of data points. We also explore why the classification accuracy was lower than expected and what can be improved in order to better the accuracy.

# 1. Introduction

This section should cover the following items:

* Motivation & Objective: Human Activity Recognition can be performed using a wide range of devices ranging from camera (video), wearable devices and radars. However, in a  real life scenario due to ease of access, privacy and robustness in data collection in various environments radars are increasingly being used.  In this project we will be using a more ambient sensing sort of approach. We will be collecting data here using an mm wave radar which generates point cloud data and accordingly perform classification on the network deployed on a Jetson nano platform. The goal here is to make an existing HAR classifier with 5 different activities real time and optimize the different blocks in the pipeline to run it on the limited hardware available.
* State of the Art & Its Limitations: Here we will basing our implementation on the RadHAR framework. This framework uses a sliding window for time and accumulates point cloud data using the mmwave radar. Using preprocessing techniques this data is voxelized and later fed to different classifiers to compare and identify which of these maximises efficiency. The existing framework collects data therefore recording and storing it in a database before the entire chunk undergoes preprocessing and training. Classification also happens on the group of test obtained during data collection. The current project however aims to make this  realtime and hence each data point collected will be individually voxelised and classified for a successful implementation.
* Novelty & Rationale: Our project picks up from what was last implemented by the framework RadHAR. This HAR model uses point clouds generated from the mmwave radar. In this project we will be using classifiers implemented previously and trained on a pre collected database. However, by generating live data from the radar, preprocessing it, and optimizing the models before trying to deploy it on the Jetson platform, we are trying to make the implementation real time to detect live actions. We will be trying to maximize test accuracy by retraining the model with newly obtained data before testing it on live data.
* Potential Impact: If successful this device could be placed in environments where monitoring needs to be done on a group, for example, in a daycare for toddlers, at an old age home for elderly people or in hospitals for patients. It can also be effectively used to track workouts. In such scenarios camera-based could be another option, but this might end up invading privacy, thus making the users uncomfortable. More importantly, these devices are agnostic to lighting conditions. Therefore, they are more efficient than vision techniques, where lighting contitions are very important factors to maintain accuracy. Hence radars can be efficiently utilized with contactless sensing in such places since it collects a very minimal data to train the networks. In most of the applications mentioned above, It is essential that the activity is interpretted or classified real-time. Hence, our project focusses on implemeting the already existing model in real-time to make these algorithms most effective. We will also try to reduce the latency of the predictions and ensure better efficiency
* Challenges: What are the challenges and risks?
* Requirements for Success: 
- Less Latency for prediction.
- Maintain the accuracy within the same accuracy margin as metioned in the offline implementation of the model.
- Ease to run the module with limited resources and computing power on jetson
- Seamless commulnication interface between the MMWAVE Radar and Nvidia Jetson module to extract the required communication.
* Metrics of Success: Latency could be measured in time (seconds) delay from the point of collection till the classification is obtained. Percentage accuracy with the right and wrong predictions can also be measured.

# 2. Related Work

# 3. Technical Approach

The setup contains the following components –
![WhatsApp Image 2022-12-10 at 10 11 45](https://user-images.githubusercontent.com/100503618/206874531-4326c39f-f0a1-4be6-84b9-e63d7d571c83.jpeg)

A.	MM Wave Radar – the radar used for data collection of the activities is TI’s IWR1443BOOST radar. It is a plug-in module with a UART to USB interface for control. The antenna provides various outputs like angles thus helping with detection in a 3D plane. It continuously transmits frequency modulated signals which enables us to measure range , angle , velocity and It has three transmitting antennas and four receiving. It is mounted on a height of 1.3m for collection. 

i.	Data collection on mmwave radar - Although there is pre existing data , we need to retrain the model with new data. We performed 5 different activities which include Walking, Squats, Jacks, Jumping and Boxing. Data is collected in .txt files and finally used to create voxelized representation of point clouds. While collection, each of the data is about         long. The sampling rate is        . And the data collected contains information like intensity, velocity, range and angle. 

B.	Jetson Nano platform – Post data collection, the pre processing and  classification is done on the python scripts running on the Jetson Nano developer kit .The Jetson Nano consists of a Maxwell GPU and an ARM Cortex CPU, thus having high compute power and is thus useful in data intensive or  computer vision applications. It’s connected to the radar using a USB interface. It has a Ubuntu-based Linux environment and for this application other dependencies like python and TensorRT have been installed. For both Data Preprocessing and classification, we’ve used the code developed by the RadHAR authors with slight modifications.

i.	Data Preprocessing – Here the point clouds are converted to voxels (i.e. dimensions 10*32*32). The value here of a voxel is the number of data points which are within the boundaries. The activities performed  - talk about window size -
-talk about samples obtained and time stamps -

ii.	Classifiers – Although RadHAR tried deriving efficiency and testing on  about four different models , for our project we’ve chosen the model with the highest efficiency. This is the Time – distributed CNN + Bidirectional LSTM classifier. Although pre trained HDF5 models for this were available, on prediction with these models, using the live data efficiency is low. Hence we retrained this module which consisted of 3 time distributed convolution modules and had a bi directional and output layer. Model was saved after    epochs.


# 4. Evaluation and Results

# 5. Discussion and Conclusions

# 6. References
