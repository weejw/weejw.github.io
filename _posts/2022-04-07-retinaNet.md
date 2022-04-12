---
layout: post
title: RetinaNet
subtitle: RetinaNet이 뭘까? 
author: weejw

categories: Thesis

tags: [Computer vision, Thesis, Object Detection, Machine Learning]
---

## RetinaNet이란? 
모델이 예측하기 어려운 hard example에 집중하도록 하는 focal Loss를 제안하였으며 Faster R-CNN 속도를 능가함 <br>
ResNet-FPN을 back으로 하여 sub-network를 2개 사용한다. (obejct classfication, bounding box regression)


## class imbalance
Yolo같은 모델은 속도는 빠르나 정확도가 낮은데 그 이유를 객체와 배경 클래스의 불균형에서 찾았다.(foreground, background class imbalance) <br>
two-stage model은 휴리스틱 샘플링이나 OHEM 기법으로 이러한 class imbalance를 해소한다.
* 휴리스틱 샘플: positive, negative 비율을 정해서 샘플링
* OHEM: 이미지에서 추출한 모든 Rols(Region of Interest)를 forwad pass한 후 loss를 계산하여, 높은 loss를 가지는 Rols에 대해서만 backward pass를 수행하는 방법([출처]())
  * forward pass: 입력값을 받아서 loss값을 구하기까지 계산해가는 과정
  * backward pass: forward pass가 끝 난 이후 역미분을 통해 기울기 값을 구해가는 과정

## Focal Loss
loss function을 수정해서 예측하기 쉬운 easy example에는 0에 가까운 loss를 부여하고 예측이 어려운 negative example에는 기존보다 높은 loss를 부여함 (scale factor)
즉, easy example의 영향을 낮추고 hard example의 영향을 높이는 것이 목표다.
아래가 focal loss 함수이다. <br>
![image](https://user-images.githubusercontent.com/33684393/162138491-c0c74b4c-de25-4d76-830d-337bb9aa5752.png)

example이 잘못 분류 된 경우 위의 pt는 낮은 값을 갖고 loss는 가중치에 영향을 받지않으며 예측하기 쉬운경우에는 pt가 큰 값을 가지게되고 loss도 0에 가까운 값을 갖는다.<br>







