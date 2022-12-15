# Abstract

This project implements a human activity recognition system in order to classify data into five different classes namely -boxing , jacks, jumping, squats and walking. In the implementation, point cloud data is obtained using the mmwave radar. This is in turn voxelised and classified using a CNN-LSTM model to obtain the output. Although several models exist for this classification, our goal is to obtain live data and deploy the CNN_LSTM model on a Jetson nano platform to provide an output with the live data. Post setting up the entire pipeline, we could successfully obtain live data from the radar and were able to interface it with the Jetson nano module provided to perform voxelisation and classification individually to each instance. We also explore why the classification accuracy was lower than expected on the live data obtained.

# Team

* Nithin Varma Adduri (005851269)
* Amulya Shruthi Tammireddi (505626283)

# Required Submissions

* [Proposal](proposal)
* [Final Presentation Slides](https://docs.google.com/presentation/d/1LEueKFinSaYRZRIE4L3bn9PwPoZocdIhcAe1IjP5ahc/edit#slide=id.g1b267e48dc1_0_3450)
* [Final Report](report)
* [Project Repo](https://github.com/nithinvarma16/M202_Project_Repo)
* [Demo Videos](https://drive.google.com/drive/u/1/folders/1hHJo0thuWHT42DP0pKj5_GDLLncPNVTY)
