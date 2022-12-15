# Project Proposal

## 1. Motivation & Objective

Human Activity Recognition can be performed using a wide range of devices ranging from camera (video), wearable devices and radars. However, in a real life scenario due to ease of access, privacy and robustness in data collection in various environments radars are increasingly being used. In this project we will be using a more ambient sensing sort of approach. We will be collecting data here using an mm wave radar which generates point cloud data and accordingly perform classification on the network deployed on a Jetson nano platform. The goal here is to make an existing HAR classifier real time and run the different blocks in the pipeline on the limited hardware available.

## 2. State of the Art & Its Limitations

Over the years, radars have been commonly used as non-contact sensing devices , some of the examples being UWB radars, Continuous Wave radars, Wifi routers and MM wave radars. Here we will basing our implementation on the RadHAR framework. This framework uses a sliding window for time and accumulates point cloud data using the mmwave radar. Using preprocessing techniques this data is voxelized and later fed to a classifier to compare and identify which of these maximises efficiency. The existing framework collects data therefore recording and storing it in a database before the entire chunk undergoes preprocessing and training. Classification also happens on the group of test obtained during data collection. The current project however aims to make this realtime and hence each instance of data collected will be individually voxelised and classified into an action for a successful implementation.

## 3. Novelty & Rationale

Our project picks up from what was last implemented by the framework RadHAR. This HAR model uses point clouds generated from the mmwave radar. In this project we will be using classifiers implemented previously and trained on a pre collected database. However, by generating live data from the radar, preprocessing it, and running the model on the Jetson platform, we are trying to make the implementation real time to detect live actions. We will be trying to maintain the previous test accuracy of the model with newly obtained data by testing it on live data.


## 4. Potential Impact

If successful this device could be placed in environments where monitoring needs to be done on a group, for example, in a daycare for toddlers, at an old age home for elderly people or in hospitals for patients. It can also be effectively used to track workouts. In such scenarios camera-based sensing could be another option, but this might end up invading privacy, thus making the users uncomfortable. More importantly, these devices are agnostic to lighting conditions. Therefore, they are more efficient than vision techniques, where lighting contitions are very important factors to maintain accuracy. Hence radars can be efficiently utilized with contactless sensing in such places since it collects a very minimal data to train the networks. In most of the applications mentioned above, it is essential that the activity is interpreted or classified real-time. Hence, our project focuses on implementing the already existing model real-time to make these algorithms utilisable in such applications. We will also try to maintain the previous accuracy of the model.


## 5. Requirements for Success
- Making the model online instead of offline
- Maintain the accuracy within the same accuracy margin as mentioned in the offline implementation of the model.
- Ease to run the module with limited resources and computing power on jetson
- Seamless communication interface between the MMWAVE Radar and Nvidia Jetson module to extract the required communication.

## 6. Metrics of Success
By making the model online instead of offline, it can be utilised in real-time applications like those discussed earlier. 
Prediction accuracy should be recorded to compare it with the original model.

## 7. Related Work

### 7.a. Papers

Papers - Akash Deep Singh, Sandeep Singh Sandha, Luis Garcia, and Mani Srivastava. 2019. RadHAR: Human Activity Recognition from Point Clouds Generated through a Millimeter-wave Radar. In Proceedings of the 3rd ACM Workshop on Millimeter-wave Networks and Sensing Systems (mmNets'19). Association for Computing Machinery, New York, NY, USA, 51–56. https://doi.org/10.1145/3349624.3356768

### 7.b. Datasets

 Datasets used – Original data present on https://github.com/nesl/RadHAR/tree/master/Data

### 7.c. Software
Various software used during the course of the project include:
- Mmwave sdk
- Ti mmwave data visualizer v2.1.
- TI code composer studio.
- Jetson developer sdk
- Jetson flash software
- Python : Keras, tensorflow

## 8. References

1. Akash Deep Singh, Sandeep Singh Sandha, Luis Garcia, and Mani Srivastava. 2019. RadHAR: Human Activity Recognition from Point Clouds Generated through a Millimeter-wave Radar. In Proceedings of the 3rd ACM Workshop on Millimeter-wave Networks and Sensing Systems (mmNets'19). Association for Computing Machinery, New York, NY, USA, 51–56. https://doi.org/10.1145/3349624.3356768.
2. D. Salami, R. Hasibi, S. Palipana, P. Popovski, T. Michoel and S. Sigg, "Tesla-Rapture: A Lightweight Gesture Recognition System from mmWave Radar Sparse Point Clouds," in IEEE Transactions on Mobile Computing, doi: 10.1109/TMC.2022.3153717.
