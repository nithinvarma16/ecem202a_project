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
* State of the Art & Its Limitations: Here we will basing our implementation on the RadHAR framework. This framework uses a sliding window for time and accumulates point cloud data using the mmwave radar. Using preprocessing techniques this data is voxelized and later fed to different classifiers to compare and identify which of these maximises efficiency. The existing framework collects data therefore recording and storing it in a database before the entire chunk undergoes preprocessing and training. Classification also happens in a group on the test data obtained during data collection. The current project however aims to make this  realtime and hence data points for each activity collected will be individually voxelised and classified for a successful implementation.
* Novelty & Rationale: Our project picks up from what was last implemented by the framework RadHAR. This HAR model uses point clouds generated from the mmwave radar. In this project we will be using classifiers implemented previously and trained on a pre collected database. However, by generating live data from the radar, preprocessing it, and optimizing the models before trying to deploy it on the Jetson platform, we are trying to make the implementation real time to detect live actions. We will be trying to maximize test accuracy by retraining the model with newly obtained data before testing it on live data.
* Potential Impact: If successful this device could be placed in environments where monitoring needs to be done on a group, for example, in a daycare for toddlers, at an old age home for elderly people or in hospitals for patients. It can also be effectively used to track workouts. In such scenarios camera-based could be another option, but this might end up invading privacy, thus making the users uncomfortable. More importantly, these devices are agnostic to lighting conditions. Therefore, they are more efficient than vision techniques, where lighting contitions are very important factors to maintain accuracy. Hence radars can be efficiently utilized with contactless sensing in such places since it collects a very minimal data to train the networks. In most of the applications mentioned above, It is essential that the activity is interpretted or classified real-time. Hence, our project focusses on implemeting the already existing model in real-time to make these algorithms most effective. We will also try to reduce the latency of the predictions and ensure better efficiency
* Challenges: What are the challenges and risks?
* Requirements for Success: 
  - Less Latency for prediction.
  - Maintain the accuracy within the same accuracy margin as metioned in the offline implementation of the model.
  - Ease to run the module with limited resources and computing power on jetson
  - Seamless commulnication interface between the mmWave Radar and Nvidia Jetson module to extract the required communication.
* Metrics of Success: 
- Latency needs to be measured in time (seconds) delay from the point of collection till the classification is obtained. 
- Percentage accuracy with the right and wrong predictions should also be measured at the output.

# 2. Related Work
Human activity recognition is now performed using a variety of acquisition devices ranging from smartphones sensors, wearable devices, and video cameras. As the applications utilizing this information are growing, so is the AI behind it to extract more information. As more sensing technologies for HAR come into picture, optimizing the networks deployed and improving the reliability of the sensing devices are always actively researched. Identifying user’s actions can be used actively to provide them with assistance. Human activity recognition is broadly classified into – vision based, and sensor based. Since vision based uses a lot of CV techniques, it could have issues like privacy leaks and occlusion making it the unfavorable choice in such situations. Hence in such cases, sensor-based methods like accelerometers, acoustic sensors and radars are used.|

Radar based HAR has slowly drawn attention for multiple reasons – one of them being privacy since it collects data that does not directly evade user privacy. It is also robust and can work in harsh conditions. Another reason is it’s ease for use, since it doesn’t have to be attached to the person and can be used in a group as well. Amongst different radars, those commonly used for HAR include –

- Continuous Wave radar – This emits stable frequency continuous wave signal. It has low power consumption and can be extended to use for portable applications. A few CW systems include – Doppler radar (acquires Doppler/ radial velocity of targets) , FMCW radar (provides range, speed info of target) and Interferometry radar ( provides angular velocity of target).
- UWB radar - This provides fine resolution of range and can identify major scattering centers of target.

The mmwave radar consists of a FMCW transceiver. The TI’s mmwave radar range is a popular one. Point clouds having the x,y,z coordinates in a 3D plane are the outputs which are given by the sensors of the mmwave radar. Increasing the bandwidth in turn increases the range resolution (i.e., ability of radar to differentiate between two targets). Since the mmwave operates in a range between 30 - 300GHz, they’re smaller is size (inversely proportional to frequency).

# 3. Technical Approach

The setup contains the following components –
![WhatsApp Image 2022-12-10 at 10 11 45](https://user-images.githubusercontent.com/100503618/206874531-4326c39f-f0a1-4be6-84b9-e63d7d571c83.jpeg)

A.	MM Wave Radar – the radar used for data collection of the activities is TI’s IWR1443BOOST radar. It is a plug-in module with a UART to USB interface for control. The antenna provides various outputs like angles thus helping with detection in a 3D plane. It continuously transmits frequency modulated signals which enables us to measure range , angle , velocity and It has three transmitting antennas and four receiving. It is mounted on a height of 1.5m for collection. 

i.	Data collection on mmwave radar - We used the pre-recorded data that was collected for radhar to train our network. This data included 5 different activities which include Walking, Squats, Jacks, Jumping and Boxing in a single environment. Data is collected in .txt files and contains information like intensity, velocity, range and angle. This data recorded was aquired at a sampling rate of 30Hz.

ii. Real-time data parsing- As our implementation is foccused on real-time classification on data recieved from mmwave radar, It is very important that we parse the data recieved from mmwave radar. In radhar, for the purpose of data collection, TI ROS package interface was used. This interface collects the data from mmwave radar and send ros packets which include measured range , angle , velocity. These ROS packets are stored in .txt format which is later used to train the network after voxeliation.
However, for our implementation since minimizing latency is the key, we have bypassed the ROS interface by directly extracting  point cloud data from the serial interface of the mmwave radar. For this purpose we have tried to understand the frame of referece that is used in the ROS interface and then we tried to implement the same from live serial data. To understand the packet structure of the mmwave radar we used the reference guides and reference mannuals provided by TI. We then parsed the input packets according the packet structure to extract the required point cloud data. The obtained point-cloud data is then pre-processed (voxelized) for the network. The data collected is sampled at 30Hz and a 2 sec sliding window used to maintain a temporal relation.

<a href="url"><img src= "https://user-images.githubusercontent.com/100503618/206874989-71a80173-154f-4f72-84f5-8f6ba556f4ef.jpeg" width=35% hieght=35%></a>
<a href="url"><img src= "https://user-images.githubusercontent.com/100503618/206874992-aa315f86-de4c-4dcd-ad9d-6ea9b43cc909.jpeg" width=35% hieght=35%></a>

B.	Jetson Nano platform – Post data collection, the pre processing and  classification is done on the python scripts running on the Jetson Nano developer kit .The Jetson Nano consists of a Maxwell GPU and an ARM Cortex CPU, thus having high compute power and is thus useful in data intensive or  computer vision applications. It’s connected to the radar using a USB interface. It has a Ubuntu-based Linux environment and for this application other dependencies like python and TensorRT have been installed. For both Data Preprocessing and classification, we’ve used the code developed by the RadHAR authors with slight modifications.

i.	Data Preprocessing – Here the point clouds are converted to voxels (i.e. dimensions 10*32*32). The value here of a voxel is the number of data points which are within the boundaries. The activities performed  - talk about window size -
-talk about samples obtained and time stamps -

ii.	Classifiers – Although RadHAR tried deriving efficiency and testing on  about four different models , for our project we’ve chosen the model with the highest efficiency. This is the Time – distributed CNN + Bidirectional LSTM classifier. Although pre trained HDF5 models for this were available, on prediction with these models, using the live data efficiency is low. Hence we also tried to retrained this module which consisted of 3 time distributed convolution modules and had a bi directional and output layer, but ran into a couple of issues which are discussed in depth later


# 4. Evaluation and Results
Post the setup and trying to get both the sides of the pipeline working, we could successfully obtain live data from the radar and were able to interface it with the Jetson nano module provided to perform voxelisation and classification individually to each set of data points for a single activity. The latency was about 3s. However there was a dip in prediction accuracy and post a session, only about 10% percent of the activites were predicted correctly. 
While trying to delve deeper into what could possibly cause a drop in the prediction accuracy of the CNN-LSTM model which originally had an accuracy of 90+ percent, we tried to verify the following parts of the pipeline -
1. Since the main difference from the original implementation was that live data was processed one activity at a time instead of the complete set of data pummped in for classification at once, the same was replicated on known data.  By serially pumping in data for one activity at a time from the existing RadHAR test dataset, we tried to verify if the output predicted is right and the accuracy for this turned out to be really high.
2. We also tried to verify if the preprocessing part of the pipeline was happening properly. Using known RadHAR test data we verified the output that we got post voxelisation while pumping in data one at a time versus the preprocessed data that was already present in a chunk. Since both these results were same as well, this part of the pipeline was also functioning alright.

# 5. Discussion and Conclusions

Therefore since the hardware/ software setup and interfacing was done right, the main part that could be causing the issue is the kind of data pumped in live. 
- Mismatch in the config file used while data collection is done using the mmwave radar:
- mismatch in physical setup could also be a contributing factor as the height and angle of the mmwave radar is not very accurate with the training data.
- The CNN_LSTM pretrained model could have been trained only on limited environments and people.

Since we have verified that the pipeline for live data pre-processing is correct, we are under the assumption that the drop in accuracy could be improved by re-training the network with appropriate dataset. Since, the main agenda of the project is to improve the real-time processing of the data, we explored other pre-processing techniques that could minimize the latency. Since voxelization with a sliding window of 2 seconds is the pre-processing technique used in Radhar, this leads to an input of dimensions (60x32x32x10) out of which most of the data is not very useful. Hence we explored other pre-processing techniques which were discussed in tesla-rapture, a HAR classifier also built for real-time classification of real-time data from mmwave radar.

# 6. Challenges faced 
We ran into the following issues while trying to configure the hardware provided and software  -

- Jetson module wasn't entering recovery mode in order to flash the system with the OS binary provided online : Flashing had to be done via the Nvidia SDK manager provided after the jetson nano platform entered into the recovery mode. Even after the right jumper and hardware settings though, the module wasn't detected by the host machine to flash the OS binary provided. Another method we used was the bootable USB one , however since the Jetson nano was an older A02 version , it didn't have support for the same. Only the B01 version supported using bootable USB to reset the module. Hence, eventually we had to follow another method using the microSD card provided and an etcher software to write the image to the SD card to eventually get the Jetson module up.
- The preprocessing block voxels.py run on a single class of input data failed stating - session crashed after using all available RAM : Here preprocessing run on any class seperately would abort with a memory error every time an array with shape (1200, 60, 10, 32, 32) of the float64_t data type was trying to be mem-allocated. The first dimension here shouldn't increase past 1200. Hence we tried running preprocessing on smaller chunks of data by dividing input data of each class into 5 sub folders. Thus every time the preprocessing was done for a sub folder, the RAM was cleared and the issue was resolved with smaller .npz files.
- Parsing the real-time point cloud data from the mmwaveradar was also challenging since all the demo applications record data into .txt file format. Since the data structure of the out-of-the box demo was not publically available in their documention folder, it took us a lot of time to figure out the packet structure.

# 7. References
1. Akash Deep Singh, Sandeep Singh Sandha, Luis Garcia, and Mani Srivastava. 2019. RadHAR: Human Activity Recognition from Point Clouds Generated through a Millimeter-wave Radar. In Proceedings of the 3rd ACM Workshop on Millimeter-wave Networks and Sensing Systems (mmNets'19). Association for Computing Machinery, New York, NY, USA, 51–56. https://doi.org/10.1145/3349624.3356768
2. D. Salami, R. Hasibi, S. Palipana, P. Popovski, T. Michoel and S. Sigg, "Tesla-Rapture: A Lightweight Gesture Recognition System from mmWave Radar Sparse Point Clouds," in IEEE Transactions on Mobile Computing, doi: 10.1109/TMC.2022.3153717.
