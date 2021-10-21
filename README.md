# WhisperNet
In this repository, I explain the implementation of my proposed network, WhisperNet, which can be used for the purpose of visual-only Lip-Based Biometric Authentication (LBBA). WhisperNet is a Siamese network which uses triplet loss as the loss function and consists of three identical embedding networks that produce representations of the input images. The siamese network then decides whether to authenticate the person based on these representations. Details of each step of the processes are explained below.

## 1. Dataset:
We used CREMA-D dataset which consists of 7,442 videos of 91 different people uttering 12 phrases under 6 different emotional states including neutral, happy, sad, anger, disgust, and fear.
## 2. Pre-Processing:
As we intend to authenticate the person based on their lip video, we should first extract the lip ROI from the input video frames. To do this task, we first used [Face Recognition](https://github.com/ageitgey/face_recognition "Face Recognition") network to extract the lip landmarks from each frame of the input video. This network outputted 24 different coordinates representing the lip region and hence, could be used to crop the Lip ROI from each frame. The output of this network is represented below:

![landmarks](https://github.com/ab2llah/WhisperNet/raw/main/cropped_How1.avi.gif "landmarks")

**Note:**
