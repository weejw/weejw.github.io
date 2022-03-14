---
layout: post
title: Object Detection Stage
subtitle: The difference between 1-Stage and 2-Stage 
author: weejw
categories: Data Processing

tags: [Obejct Detection, TIL]
---
1. 2-stage Detector: Regional Proposal/ Classification을 순차적으로 진행함.<br>
2. 1-Stage Detecotr: Regional Proposal/ Classification을 동시에 진행함.

속도와 정확도에 장단점을 갖음. 정확도가 높기 위해선 2-Stage가 속도를 높이기 위해선 1-Stage가 좋은 것.

- Regional Proposal: 기존 sliding window방식을 효율적으로 구성하기 위해 물체가 존재할 법한 영역을 빠르게 찾아내는 알고리즘

### REFERENCES

https://www.secmem.org/blog/2021/06/20/Object_Detection/﻿
