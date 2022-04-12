---
layout: post
title: Object tracking
subtitle: Object tracking 방법에 대해 탐구한다. 
author: weejw
categories: Machine-Learning

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


## 오츠의 이진화 알고리즘
[이 블로그](https://bkshin.tistory.com/entry/OpenCV-8-%EC%8A%A4%EB%A0%88%EC%8B%9C%ED%99%80%EB%94%A9Thresholding) 에 의하면 오츠의 진화 알고리즘은 thresholding 최적값을 자동으로 탐색해준다고 한다.<br>
임의의 임곗값으로 시작해 두 부류의 명암 분포를 구하는 작업을 반복하다가 이 분포가 가장 균일할 때의 임곗값을 선택한다.
결과는 아래와 같다. 오츠의 알고리즘에 의하면 최적의 임곗값은 95이다.

![2022-03-21-obj-tracking-img-6](https://user-images.githubusercontent.com/33684393/159228448-2e6c0f61-96ee-4940-a3c7-730eb69516df.png)

## 적응형 스레시홀딩
<br> 그러나 일정하지 않은 조명, 배경색이 여러개일덴 이 것이 잘 맞지않는다고한다. 
그리하여 사용되는 것이 적응형 스레시홀딩이다. 이는 이미지를 영역으로 나눠 주변 픽셀값만을 이용해 힘곗값을 구한다.<br>
결과는 아래와 같다. 우측 상단이 전역 스레시홀딩에서 발생하는 전형적인 문제라고한다.  
(* 전역 스레시홀딩: 임곘값을 넘으면 255/ 그렇지않으면0) 그늘 등으로 인해 발생한다.<br>
적응형 스레시 홀딩을 적용한 아래 두 이미지는 상당히 선명하다.<br>
좌측 하단은 평균값을 이용한 것이고 우측 하단은 가우시안 분포를 활용한 것이다. 평균값을 활용한 것이 선명도가 높고 노이즈가 비교했을 때 있으며 가우시안은 선명도는 떨어지나 노이즈는 적다.<br>
대부분의 영상, 이미지는 그림자를 지니고있기때문에 전역 스레시홀딩이 아닌 적응형 스레시홀딩을 많이 쓴다고한다.

![2022-03-21-obj-tracking-img-7](https://user-images.githubusercontent.com/33684393/159228450-0c48c115-7c54-45b4-a71a-f87fb725a20b.png)




<br><3/23일 추가>

[이 곳](https://riptutorial.com/opencv/example/29159/canny-edge-thresholds-prototyping-using-trackbars) 을 참고해서 코드를 track bar 기반으로 만들었다. 으흐흣.. 조절을 바로 할 수 있으니 좋다.

![Hnet-image](https://user-images.githubusercontent.com/33684393/159423302-44b1c17c-e3e7-4b95-887f-3b6428df99ce.gif)



## REFERENCES
[https://engineer-mole.tistory.com/243](https://engineer-mole.tistory.com/243)<br>
[https://crmn.tistory.com/50](https://crmn.tistory.com/50)

