---
layout: post
title: Data Augmentation
subtitle: 데이터가 부족할 땐? Data Augmentation!
author: weejw
categories: Data

tags: [Data, Computer Vision, pre-prosessing]
---
 
## Data Augmentation

'augment': 증강시키다. 즉, 하나의 데이터셋을 여러가지 방법으로 증강시켜 데이터셋의 규모를 키우는 방식이다. 
종류는 Mirroring, Random Cropping, Rotation, Shearing 등이 있다. 딥러닝의 고질적인 문제 중 하나인 오버피팅을 해결하기 위해서
학습 면적을 너무 과하지않게 변화시키기 위해 사용한다. [출처](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=4u_olion&logNo=221437862590)

이미지에 인위적으로 노이즈를 주어 학습 폭을 넓히는 것인데, 과하게 주면 좋지않다고 한다. [이 글](https://hoya012.github.io/blog/Image-Data-Augmentation-Overview/) 에서 다양한 augmentaion의 종류를 예제와 함께 볼 수있다. <br>

Augmentation의 기법은 개가 있다.
- Pixel-Level: 픽셀 단위로 변환을 시킨다. blur, [jitter](https://nrhan.tistory.com/entry/Data-augmentation-color-jitter), noise
- Spatial-Level: 이미지 자체를 변환시킨다. flip, roation
