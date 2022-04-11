---
layout: post
title: Data 처리에 관련된 모든 용어
subtitle: Data 처리와 관련된 용어를 알아보자.
author: weejw
categories: python

tags: [Data]
---

### BI(Business Intelligence)
데이터 통합/분석해 기업 활동에 연관된 의사결정을 돕는 프로세스

### DL(Data Lake)
다양한 원천을 그대로 가져와 저장하여 다양성을 보존함

### Data Mesh
중앙 집중형 데이터 관리의 문제점으로 인해 생긴 분석 시스템 아키텍쳐로 Monolithic 구조를 가지고 있다.
Monolithic 구조는 분석 플랫폼이 모든 도메인에 대해 단일 시스템과 단일 분석팀을 사용한다.
즉, 업무별로 시스템과 팀을 분리하는 구조이다. 

### DW(Data Warehouse)
사용자 의사결정에 도움주기위한 데이터 베이스 집합

### Staging 로딩
source system으로부터 제공받은 데이터를 아무런 변화 없이 그대로 로딩하는 저장 공간, 임시 저장공간 성격

### EDW(Enterprise Data Warehouse)
여러 애플리케이션의 비즈니스 정보를 중앙 집중화하고 조직 전체에서 분석/사용할 수 있도록 하는 DB. 데이터 표준화, 유연성, BI 도구에대한 연결성의 이점을 가질 수 있음

### ADW(Analytical Data Warehouse)
전사 관점의 통합 분석 자료를 제공

### RDW(Real-time Data Warehouse)
원천 데이터 실시간 처리로 실시간 비즈니스 활용 기반을 구축

### DM(Data Mart)
특정 주제를 중심으로 구축된 소규모 DW

### DW와 DM을 나눈 이유는?
DM와 DW의 차이는 DW는 특정 주제를 가지고 운영된다는 것으로 DM의 필요 이유는 DW로 수많은 유저가 접근할 시 큰 부하가 생기고 원천을 크게 가공하지 않은 상태인 DW가 좋은 정보를 제공하지 못할 가능성이 있기때문이다. 쉽게 생각하면 물류창고에서 마트로 물건을 옮겼다고 생각하면 됨

### OLTP(On-Line Transaction Processing)
기업 운영에 필요한 비즈니스 프로세스를 자동화한 시스템, 은행 창구 업무 등, 이 때 시스템으로부터 데이터를 추출,수정,요약해서 의사결정을 지원할 수 있는 DB가 DW

### OLAP()
OLTP를 활용하여 데이터를 분석하고 이를 활용하는 방법

### ETL(Extraction, Trasformation, Loading)
원천 데이터로부터 데이터를 추출 및 변환하여 적재하는 구성 요소, Batch작업으로 정형 데이터 통합

### CDC(Change Data Capture)
real time으로 원천 데이터를 처리함, 로그나 통신을 통한 변경이 이루어짐
- source DB에서 target DB로 실시간 복제를 진행하는 방식으로 진행됨
- source DB에 영향을 거의 주지말아야하며, source DB의 데이터가 변경된 부분만 읽어서 정합성 유지

### ODS(Operational Data Store)
DW로 데이터를 저장하기 전에 임시로 운영 데이터를 보관하는 장소로 이력 데이터(스냅샷 데이터)를 저장함

### DW 구축 순서
1. DW modeling
2. ETL (Leagcy -> DW)
3. Data Mart Modeling
4. ETL (ODS/DW -> DM)
5. ROLAP (DW에서 ROLAP)
6. MOLAP (DM에서 MOLAP)
7. DW 시스템 운영

### DW 구축 방법
- Top-Down: 전사 관점에서 한번에 구축, 시간/비용/전사적 지원 필요
- Bottom-Down: 각 부서별로 mart 먼저 구축하고 이후 통합이라 통합에 대한 설계 고려해야함
- Hybrid: DW와 DM을 병합으로 구축 비용/인력 분산 투입가능하며 위험을 최소화 시킬 수 있음


### Data 처리 순서

Legacy -> staging -> ODS -> DW -> DM -> 필요한 곳

### References
[https://ehyun0128.github.io/miscellaneous/dm_dw_dl/](https://ehyun0128.github.io/miscellaneous/dm_dw_dl/) <br>
[https://dbrang.tistory.com/650](https://dbrang.tistory.com/650) <br>
[https://velog.io/@sezzzini/%EA%B8%88%EC%9C%B5-IT-ETL-%EA%B3%BC-DW](https://velog.io/@sezzzini/%EA%B8%88%EC%9C%B5-IT-ETL-%EA%B3%BC-DW) <br>
[https://simroot.tistory.com/25](https://simroot.tistory.com/25) <br>
[https://bcho.tistory.com/1379](https://bcho.tistory.com/1379) <br>
