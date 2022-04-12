---
layout: post
title: airflow 기반의 데이터 파이프라인
subtitle: airflow data pipeline book study 
author: weejw
categories: Data-Pipeline

tags: [Data, Airflow, Data Pipeline]
---



출시하자마자 책을 샀는데 이제서야 들여다본다. 상반기내로 이 책을 끝내길 바란다. ~~근데 개인적으로 책 홍보이미지 너무 ㅋㅋ 무슨 분양글인줄 알았다.~~<br>
<img src="https://user-images.githubusercontent.com/33684393/160342956-5998c0e3-9900-49b6-942b-695b022e94bb.png" width="200" height="200">    <img src="https://user-images.githubusercontent.com/33684393/160343314-ebbc7c68-1079-4c9a-86d5-c84971886d59.png"  width="300" height="400"> <br>

** 모든 예제 코드는 [https://github.com/K9Ns/data-pipelines-with-apache-airflow](https://github.com/K9Ns/data-pipelines-with-apache-airflow) 에 있다. 미리 clone 해두자.. 물론 직접 타이핑하는 것도 좋고~ 


## Airflow, 왜써야해?
- 확장성
- python 기반
- 다양한 스케쥴링 기법
- 백필 기능
- 나이스한 web UI

## Airflow, 이럴땐 쓰지말아야돼!
- Streaming processing
- 추가 및 삭제가 빈번한 동적 파이프라인
- python을 모를 때

## 데이터 수집해보자 

[https://thespacedevs.com/llapi](https://thespacedevs.com/llapi) 이 api를 사용하는데, 보면 아래와 같은 엔드포인트가 존재한다.

![image](https://user-images.githubusercontent.com/33684393/160344821-45ff562b-a6a3-4b50-b3ac-545e712cdc30.png)

데이터는 위 api를 이용해 수집하며 그 코드는 깃헙에 있다. <br>
다음은 깃헙 소스의 chapter02/dags/download_rocket_launches.py 코드의 일부다. 이 외에는 BashOperator나 PythonOperator 등이 있다. 기초기때문에 숙숙 지나가겠다.

```python
dag = DAG(
    dag_id="download_rocket_launches",
    description="Download rocket pictures of recently launched rockets.",
    start_date=airflow.utils.dates.days_ago(14), # 시작날짜
    schedule_interval="@daily", # 시작 주기
)
```

## Task와 Operator의 차이가 뭘까?

- Airflow에서 Task는 작업의 올바른 실행을 보장하기 위한 <u>오퍼레이터의 Wrapper 또는 Manager이다.</u> <br>
- Airflow는 Task를 통해 작업을 올바르게 실행할 수 있다. <br>
- Task는 Operator의 상태를 관리하고 사용자에게 상태 변경을 표시하는 airflow의 내장 컴포넌트이다.
- DAG는 오퍼레이터 집합에 대한 실행을 오케스트레이션하는 역할을 한다.


## 코드를 실행해보자!

위 깃헙에있는 코드를 실행해보자. 그 전에 일단 필요한 것부터..
```shell
$ conda create -n airflow
$ pip install apache-airflow
$ airflow db init
$ airflow users create \
          --username admin \
          --firstname FIRST_NAME \
          --lastname LAST_NAME \
          --role Admin \
          --email admin@example.org
$ cp github_repo/download_rocket_launches.py airflow/dags
$ airflow webserve -D
$ airflow scheduler -D
```

그리고 나서 localhost:8080으로 진입하면 아무 문제없이 화면을 띄울 수 있다. 맨 위에 새로 추가한 DAG가 보인다. 실행해보자. 실행도 문제없다..
![image](https://user-images.githubusercontent.com/33684393/160352516-6395932d-c1c2-49c1-b863-ce7577912171.png) <br>
![image](https://user-images.githubusercontent.com/33684393/160353191-1687dd08-388c-40bc-9e79-281f2a99976e.png) <br>
![image](https://user-images.githubusercontent.com/33684393/160353300-ceab6a4b-2fd2-4bb2-9370-037ccf6ae7db.png) <br>

## 도커로 실행해보기

깃헙 소스코드에 보면 script 폴더 아래에 docker로 실행하는 명령어가 저장되어있다. 이를 진행해보자.<br>
```shell
$ docker run \
-it \
-p 8080:8080 \
-v /home/weejw/airflow/data-pipelines-with-apache-airflow/chapter02/dags/download_rocket_launches.py:/opt/airflow/dags/download_rocket_launches.py \
--name airflow \
--entrypoint=/bin/bash \ 
apache/airflow:2.0.0-python3.8 \
-c '( \
airflow db init && \
airflow users create --username admin --password admin --firstname Anonymous --lastname Admin --role Admin --email admin@example.org \
); \
airflow webserver & \
airflow scheduler \
'
```

전혀 문제없이 실행된다 :) image 파일도 도커내에 생성된 것을 확인할 수있었다. <br>
![image](https://user-images.githubusercontent.com/33684393/160355987-fc768e11-4aff-4d40-95d0-e0de418757e7.png) <br>

```shell
(airflow) weejw@DESKTOP-LQRSEG1:~$ docker exec -it airflow /bin/bash
airflow@02ca7330e8cf:/opt/airflow$ ls
airflow-webserver.pid  airflow.cfg  airflow.db  dags  logs  unittests.cfg  webserver_config.py
airflow@02ca7330e8cf:/opt/airflow$ cd /tmp/images/
airflow@02ca7330e8cf:/tmp/images$ ls
electron_image_20190705175640.jpeg        long2520march252011_image_20190222031217.jpeg
falcon2520925_image_20210310075447.jpeg   long_march_6_image_20210709074933.jpg
falcon2520925_image_20220308100515.jpeg   new2520shepard_image_20190207032624.jpeg
falcon2520925_image_20220324065634.png    zhuque-2_image_20200905210848.jpg
falcon_9_block__image_20210506060831.jpg
```
