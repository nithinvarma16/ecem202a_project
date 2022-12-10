# Abstract

This project implements a human activity recognition system in order to classify data into five different classes namely -boxing , jacks, jumping, squats and walking. In the implementation, point cloud data is obtained using the mmwave radar. This is in turn voxelised and classified using a CNN-LSTM model to obtain the output. Although several models exist for this classification, our goal is to obtain live data and deploy the optimised and most efficient model on a Jetson nano platform to provide an output with the least latency. Post setting up the entire pipeline, we could successfully obtain live data from the radar and were able to interface it with the Jetson nano module provided to perform voxelisation and classification individually to each set of data points. We also explore why the classification accuracy was lower than expected and what can be improved in order to better the accuracy.

# Team

* Nithin Varma Adduri (005851269)
* Amulya Shruthi Tammireddi (505626283)

# Required Submissions

* [Proposal](proposal)
* [Midterm Checkpoint Presentation Slides](http://)
* [Final Presentation Slides](http://)
* [Final Report](report)
