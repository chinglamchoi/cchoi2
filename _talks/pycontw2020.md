---
title: "[PyCon Taiwan 2020] Corona-Net: Fighting COVID-19 with Machine Learning"
<img src="https://chinglamchoi.github.io/cchoi/files/pycontw2020.PNG">  
collection: talks
type: "Talk"
permalink: /talks/pycontw2020
venue: "PyCon Taiwan 2020"
date: 2020-09-06
location: "Remote"
---
![PyCon Taiwan 2020 Talk](https://chinglamchoi.github.io/cchoi/files/pycontw2020.PNG)    
[More information here](https://tw.pycon.org/2020/en-us/conference/talk/1163063448498602356/)

**Abstract**
In this talk, I will introduce my open source Python project, Corona-Net, COVID-19 chest CT segmentation with PyTorch. I leverage the EfficientNet model for image-level COVID-19 diagnosis, as well as the UNet -- a Fully Convolutional encoder-decoder network -- for binary and multi-class medical segmentation. Through UNet, I successfully detect and localise COVID-19 symptoms, including ground-glass, consolidation and pleural effusion, from axial chest CT slices, using open source datasets and annotations.

**Description**
Identified in December 2019, the novel Coronavirus has infected 2.7 million worldwide, and claimed the lives of 0.2 million. Amidst this deadly pandemic, I started my open source project, Corona-Net, in the hopes of contributing to the global fight against the Coronavirus. Corona-Net is a 3-part project dedicated to the classification, binary segmentation and multi-class segmentation of COVID-19. I first leverage the EfficientNet model for COVID-19 diagnosis, then utilise and refine the U-Net architecture for both binary and 3-class (ground-glass, consolidation, pleural effusion) segmentation of COVID-19 symptoms, through inference on the COVID-19 CT segmentation (chest axial CT) dataset. Through Corona-Net, I aim to develop a reliable, visual-semantically balanced method for automatic COVID-19 diagnosis, as well as extend an invitation to all to collaborate and stand together against this pandemic. My PyTorch code is publicly available at [https://github.com/chinglamchoi/Corona-Net](https://github.com/chinglamchoi/Corona-Net).
