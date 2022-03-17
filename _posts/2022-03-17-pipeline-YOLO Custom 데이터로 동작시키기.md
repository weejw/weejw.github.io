---
layout: post
title: YOLO(You Only Look Once) v5 Custom 데이터로 동작시키기
subtitle: Object Detection 분야에 많이 알려진 모델 YOLO에 다른 데이터를 넣어보자 
author: weejw
categories: Machine-Learning

tags: [Obejct Detection, YOLOv5, Machine Leanring]
---
AI허브에 공개된 물고기 동영상 데이터를 이용했다. 
[https://aihub.or.kr/aidata/30735](https://aihub.or.kr/aidata/30735)

[YOLOv5 깃허브](https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data) 에 나와있는 것과같이 [roboflow](https://roboflow.com/annotate?ref=ultralytics) 를 이용해서 어노테이션을 진행했다.

물론,, 데이터 양이 많으면 당연히 좋겠지만 일단 소량으로 진행해봤다.. 너무 적나 하하..
![2022-03-17- YOLO-custom-사용해보기-img-1](https://user-images.githubusercontent.com/33684393/158744130-3845d9f4-47c3-4d21-a30f-2c35cda0f469.PNG)

으악 데이터가 너무 적었나보다.. 좀 더 라벨링이 필요하다. 
![2022-03-17- YOLO-custom-사용해보기-img-2](https://user-images.githubusercontent.com/33684393/158749719-f9c59f60-26ba-4ccf-a186-e9e637be068a.PNG)
