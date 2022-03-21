---
layout: post
title: Object tracking
subtitle: Object tracking 방법에 대해 탐구한다. 
author: weejw
categories: Machine Learning

tags: [Object tracking, Machine Learning]
---

Object tracking을 위해선 각 프레임마다 나타나는 object간 유사성 비교가 반드시필요하다.
이를 위해서는 이미 연구가 많이 되어있는 상태 [Deep Sort](https://github.com/nwojke/deep_sort), roboflow의 [CLIP](https://blog.roboflow.com/clip-model-eli5-beginner-guide/), 국내에서도 [Deep Learing을 기반으로 한 Feature Extraction 알고리즘의 분석](https://www.koreascience.or.kr/article/JAKO202020455277386.pdf) 이 있다. <br>

지난번에 YOLO 모델을 돌리기위해 일일히 라벨링을 해야했다. 물론 좋은 데이터의 품질을 위해선 이런 라벨링이 필수적이라고 생각하지만 조금 더 수월하게 진행할 방법이 없을까? 라는 생각이 들었다.<br>

너무 어렵게 접근하려는 것 같다는 생각이 문득 들었다.<br> 
YOLO 모델까지도 가지않고 정확히 feature extraction => tracking만 우선적으로 진행하는 방안을 찾아야했다.<br>
[https://github.com/alexkychen/Auto-Fish-Counter](https://github.com/alexkychen/Auto-Fish-Counter) 이 깃 허브에 가면 짧고 간결하게 코드가 작성되어있었다.<br>


내가 필요한 부분은 아래 단 몇 줄이었다. 위에 한국어 논문에도 나와있는 과정과 유사해보였다.<br>
결론적으론 contours를 찾으려 Canny 엣지를 추출한다. 
```python
    for frame in camera.capture_continuous(rawCapture, format='bgr', use_video_port=True):
        image = frame.array
        grayimage = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
        blurimage = cv2.GaussianBlur(grayimage, (blursize, blursize), 0)
        T, threshimage = cv2.threshold(blurimage, threshVal, 255, cv2.THRESH_BINARY)
        edgedimage = cv2.Canny(threshimage, cannyMin, cannyMax)
        cv2.imshow('frame', image)
        (cnts, _) = cv2.findContours(edgedimage.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        fishCountNum = len(cnts)
        print("I count %d fish in this image" % fishCountNum)

```

우선 [이 블로그](https://engineer-mole.tistory.com/243) 에 기반하여 가장 기본부터 진행하기위해 gray scale로 변환한 뒤 canny 작업을 진행했다.(위 논문에 의하면 이를 2차 미분에 의한 엣지 추출로 정의한다)<br>
threshold2는 경계 유무를 판단하는 임곗값이고, threshold1은 이 경계들이 이어질지에 대한 값이라고한다. 자세한 내용은 위 블로그에 상세하게 적혀있다.<br>


![2022-03-21-obj-tracking-img-1](https://user-images.githubusercontent.com/33684393/159224773-7be75046-85f0-428f-8e56-1ae15e3beee5.png)


그리고나서 위 깃허브에 있는 코드를 가지고도 해보았다.

```python
import cv2
import matplotlib.pyplot as plt
```


```python
plt.rcParams["figure.figsize"] = (20,20)
```


```python
rgb = cv2.imread("swim_bp_2020-12-20-10-18_02-32-41_300.jpg", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(rgb, cv2.COLOR_BGR2GRAY)
```


```python
plt.subplot(1, 2, 1) # 1행 2열에서 1번째 열

plt.imshow(rgb)

plt.xticks([]) # x축 좌표 숨김

plt.yticks([]) # y축 좌표 숨김



plt.subplot(1, 2, 2) # 1행 2열에서 2번째 열

plt.imshow(gray, cmap='gray')

plt.xticks([]) # x축 좌표 숨김

plt.yticks([]) # y축 좌표 숨김



plt.show()


```


    
![2022-03-21-obj-tracking-img-2](https://user-images.githubusercontent.com/33684393/159224774-7f77ac6f-a38c-44de-9ecf-3c7aa0d83e14.png)
    



```python
blurimage = cv2.GaussianBlur(gray, (1, 1), 0)
plt.imshow(blurimage)
plt.show()
```


    
![2022-03-21-obj-tracking-img-3](https://user-images.githubusercontent.com/33684393/159224778-29f5bc3a-c050-4377-a4c8-c05e7a149d55.png)
    



```python
T, threshimage = cv2.threshold(blurimage, 65, 55, cv2.THRESH_BINARY)
plt.imshow(threshimage)
plt.show()
```


    
![2022-03-21-obj-tracking-img-4](https://user-images.githubusercontent.com/33684393/159224779-4828de93-3a04-4410-ade1-214cfc02b865.png)
    



```python
threshold1=50
threshold2=100
edgedimage = cv2.Canny(threshimage, threshold1, threshold2)
plt.title(f't1={threshold1}, t2={threshold2}')
plt.imshow(edgedimage)
plt.show()
```


    
![2022-03-21-obj-tracking-img-5](https://user-images.githubusercontent.com/33684393/159224780-96af86ea-6a9f-4a33-b271-539444089a91.png)
    



```python
(cnts, _) = cv2.findContours(edgedimage.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
fishCountNum = len(cnts)
print("I count %d fish in this image" % fishCountNum)
```

    I count 1986 fish in this image
    
검출이 생각처럼 쉽진않다. 파라미터를 조정하는 법에 대해 더 알아봐야할 듯하다.

## REFERENCES
[https://engineer-mole.tistory.com/243](https://engineer-mole.tistory.com/243)<br>
[https://crmn.tistory.com/50](https://crmn.tistory.com/50)

