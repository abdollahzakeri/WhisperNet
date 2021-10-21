# WhisperNet
In this repository, I explain the implementation of my proposed network, WhisperNet, which can be used for the purpose of visual-only Lip-Based Biometric Authentication (LBBA). WhisperNet is a Siamese network which uses triplet loss as the loss function and consists of three identical embedding networks that produce representations of the input images. The siamese network then decides whether to authenticate the person based on these representations. Details of each step of the processes are explained below.

## 1. Dataset:
We used CREMA-D dataset which consists of 7,442 videos of 91 different people uttering 12 phrases under 6 different emotional states including neutral, happy, sad, anger, disgust, and fear.
## 2. Pre-Processing:
As we intend to authenticate the person based on their lip video, we should first extract the lip ROI from the input video frames. To do this task, we first used [Face Recognition](https://github.com/ageitgey/face_recognition "Face Recognition") network to extract the lip landmarks from each frame of the input video. This network outputted 24 different coordinates representing the lip region and hence, could be used to crop the Lip ROI from each frame. The output of this network is represented below:

![landmarks](https://github.com/ab2llah/WhisperNet/raw/main/cropped_How1.avi.gif "landmarks")

**Note:** The landmarks are plotted on the input image for clarification and are not fed into the network like this.

The sequence of these extracted landmarks are saved accordingly to a TF file to be used later in the network. Additionally, they were used to crop each frame to the lip ROI.
## 3. The Embedding Network:
The embedding network takes the cropped lip video along with the corresponding sequence of landmarks as input and outputs an embedding vector. The architecture of the embedding network is represented below:
![embedding](https://github.com/ab2llah/WhisperNet/raw/main/embedding.png "embedding")
The embedding network is consisted of two branches. The right branch extracts features from the input lip video and the right one extracts features from the sequence of lip landmarks. architecture of the left branch is inspired by the LipNet and uses STCNN layers to extract spatio-temporal features from the input video.

The results of these 2 branches are concatenated and processed further to produce the final embedding of the input data.
## 4. The Siamese Network:
The Siamese network uses the triplet loss function :
`L(A,P,N)=max⁡(0,D(A,P)-D(A,N)+margin)`
where `D(x,y)` is the distance metric used to calculate the distance between x and y. *L2 distance* or *(1 – cosine similarity)* can be used as the distance metric. The margin term represents the minimum required distance between `D(A,P)` and `D(A,N)`. The architecture of the used Siamese network is represented below:

![siamese](https://github.com/ab2llah/WhisperNet/raw/main/siamese.png "siamese")

The siamese network is consisted of three identical embedding networks which produce representation vectors for the anchor, positive, and negative input data. During the process of training, the network learns to maximize the distance between the anchor and negative embeddings while minimizing the distance between anchor and positive embeddings simultaneously.

