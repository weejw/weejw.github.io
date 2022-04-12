---
layout: post
title: Object Detection Stage
subtitle: The difference between 1-Stage and 2-Stage 
author: weejw
categories: Data-Processing

tags: [Obejct Detection, Machine Learning]
---
1. 2-stage Detector: Regional Proposal/ Classification을 순차적으로 진행함.<br>
ex: R-CNN, Fast R-CNN 등
2. 1-Stage Detecotr: Regional Proposal/ Classification을 동시에 진행함.<br>
ex: YOLO, SSD 등

속도와 정확도에 장단점을 갖음. 정확도가 높기 위해선 2-Stage가 속도를 높이기 위해선 1-Stage가 좋은 것.

- Regional Proposal: 기존 sliding window방식을 효율적으로 구성하기 위해 물체가 존재할 법한 영역을 빠르게 찾아내는 알고리즘


조금 더 자세하게 들어가자면,,

1. RCNN: Region proposal algorithm(Selective search)과 이를 분류하는 CNN 알고리즘을 수행하고 Classification, Bouding Box regression을 위한 신경망 동작
2. YOLO: 이미지를 Grid로 나누고, Sliding window 기법대신 Convolution 을 사용하여 Grid Cell 별로 Bbox를 얻고 이에 NMS를 진행
* NMS(Non-maximum Suppression): YOLO에서 한 객체에 대해 Bbox가 여러가지 잡힐 수가 있는데 이때 신뢰도가 가장 높은 Bbox만 남기는 과정에 사용되는 알고리즘 

### REFERENCES
- https://www.secmem.org/blog/2021/06/20/Object_Detection/﻿<br>
- https://velog.io/@cha-suyeon/%EB%94%A5%EB%9F%AC%EB%8B%9D-Object-Detection-Architecture-1-stage-detector%EC%99%80-2-stage-detector<br>
- https://hongl.tistory.com/180
