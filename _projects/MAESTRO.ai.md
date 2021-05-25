---
title: "MAESTRO.ai TaiChi Tutor"
collection: projects
type: "Computer Vision project"
permalink: /projects/maestro
venue: "GitHub (https://github.com/chinglamchoi/MAESTRO)"
date: 2020-07-27
location: "Hong Kong"
---
![MAESTRO](https://chinglamchoi.github.io/cchoi/files/maestro.PNG)  
We introduce MAESTRO.ai TaiChi Tutor, a healthcare application which leverages state-of-the-art Human Pose Estimation technology for social good. Our contributions are two-fold: firstly, adapting the OKS Keypoint Similarity metric for body-shape invariant comparison of TaiChi postures; secondly, introducing an interactive platform for the automatic AI tutorial of TaiChi to solve various social issues.
  
Guided by our motto, "Technology Bridging Generations", we leverage the PoseNet model to combat issues of elderly depression and lack of intergenerational understanding in the aging Hong Kong society. We achieve this through the gamification and modernisation of the ancient art of TaiChi, connecting both youngsters and elders in learning and practicing TaiChi, enhancing physical and mental health through exercise, while simultaneously promoting intergenerational interaction and adhesion. 

Introduction
======
With exponential advances in technology, Hong Kong's elderly are often left behind in this digital trend. As youngsters increasingly turn to technology for work and play, elderly become more and more isolated, widening the intergenerational communication gap. With MAESTRO, we aim to bring together members of society of different ages through the ancient Chinese art of TaiChi, breaking barriers in communication whilst promoting good physical and mental health.
  
MAESTRO is designed with the objective of enhancing social inclusion and intergenerational understanding. We achieve this by tackling 2 severe issues: elderly depression and the lack of intergenerational interaction.
  
MAESTRO uses a PoseNet architecture with the MobileNetv1 backbone feature extractor to detect user pose and the OKS similarity metric to evaluate user pose. PoseNet allows us to balance speed and accuracy while OKS weighs keypoint errors instead of directly comparing values, which makes our accuracy evaluation comprehensive. PHP and mySQL are used to create the backend to our web app and HTML, CSS, and JS are used in the frontend.
  
As an ancient art which develops both mind and body, TaiChi is much beloved by communities throughout Hong Kong. It is also an inherently social sport, encouraging enthusiasts to practice routines and share knowledge together. Many elderly participate in large TaiChi practice groups which acts as a platform for learning, but more notably, socialising. TaiChi brings up many discussions, from which sets of moves you practice, to which school of TaiChi school's training philosophy you agree with the most. These benefits are not limited by age groups. Through our app, both elder and younger generations can enjoy practising the ancient art, while simultaneously promoting intergenerational interaction and adhesion. 

PoseNet
======
![PoseNet](https://chinglamchoi.github.io/cchoi/files/posenet.png)  
MAESTRO leverages the PoseNet architecture for Single-Person 2D Human Pose Estimation, mapping users' postures into human pose skeletons (by first computing heatmaps and offset vectors, then performing sigmoid and argmax functions, in order to estimate the human joint coordinates and output a human pose skeleton). Utilising the MobileNet v1 Convolutional Neural Network backbone feature extractor, PoseNet balances both detection speed and accuracy, making it ideal for our task. 

MobileNet
======
MobileNet introduces the insights of depth-wise separable convolutions and the introduction of a latency-accuracy tradeoff policy governed by 2 global hyperparameters, enabling model pruning. These modifications compose an efficient, customisable model architecture, uniquely suited for our task of real-time feature extraction for Human Pose Estimation. The computationally efficient depthwise convolution is linearly combined with a 1x1 pointwise convolution to make up the depthwise separable convolution, as defined below:
![convolutions](https://chinglamchoi.github.io/cchoi/files/convolutions.png)  
By utilising 3x3 depthwise convolutions, MobileNet reports using 8-9 times less computation with negligible accuracy degradation. Consisting 26 layers, MobileNet also enables model pruning by introducing the hyperparameter width multiplier $\alpha$ in $(0,1]$, where a setting of $\alpha < 1$ is reported to reduce computational cost and the number of model parameters by $\alpha^2$. Subsequently, hyperparameter resolution multiplier $\rho$ in $(0,1]$ is introduced to the input image and feature maps, where settings of $\rho < 1$ reduces computational cost by $\rho^2$. MobileNet's insights of using depthwise separable convolutions, as well as introducing hyperparameters $\alpha, \rho$. It causes significant reduction in computational cost, as well as offers a significant degree of customisation ability (accuracy-latency) tradeoff, making it the ideal feature extractor for MAESTRO.
  
Object Keypoint Similarity metric  
======
We adapt the OKS similarity metric, a conventionally used evaluation metric (e.g. in CoCo) for keypoint detection, to compare and evaluate the TaiChi gestures of MAESTRO users and TaiChi masters. Defined as below:
![OKS](https://chinglamchoi.github.io/cchoi/files/oks.png)  
OKS normalises joint keypoints with the Gaussian kernel, facilitating body type invariant, camera angle invariant, fair and reliable TaiChi posture comparisons. In MAESTRO, we pre-compute the keypoint coordinates for TaiChi masters, and compute keypoint coordinates of MAESTRO users in real-time, and calculate the OKS similarity score between the 2 sets of data every 2 seconds. Using the size of body parts as a weight to give an accuracy score of keypoints can make comparisons fair as the same distance may be a large error if it happens to the head while not being significant if it occurs at the ankles. The pose accuracy is judged using the best result of each keypoint per 5 evaluations so that users can react to a change in pose in the tutoring video. The keypoint with the worst score is determined and shown on screen to let the user know. The final score is calculated based on the worst score in each set of 5 evaluations.
  
Conclusion
======
Through MAESTRO, we introduce an AI TaiChi tutorial system which is accessible to all, and able to perform real-time body-shape and camera-angle invariant TaiChi pose comparisons. By leveraging PoseNet with the MobileNetv1 backbone feature extractor, adapting the OKS similarity metric, as well as designing a complementary interactive User Interface, we compose a robust TaiChi tutor application which balances both speed and accuracy.
  
Hong Kong as a technologically advanced city has let down those who struggle to catch up with the trend, exacerbating problems of elderly depression and the intergenerational communication gap. MAESTRO is the perfect solution to close the widening communication gap and promote social inclusiveness. Through MAESTRO, the younger generation will communicate with the older generation while learning about cultural traditions; the older generation can gain exposure to Artificial Intelligence and feel its usefulness in daily life. We bridge together the old and the young to revel in the joys of practising TaiChi. With MAESTRO, traditional culture and technology will no longer be at odds with each other, but work together to create a better, more inclusive Hong Kong.

