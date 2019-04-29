---
title: 데이터 마이닝
date: 2019-04-29 14:45:00
tags:
categories:
- 개발공부
---

# 데이터 마이닝

- 데이터로부터 알려지지 않은 특성(unknown properties)을 발견하는 기술에 중점

## 분석기법

### 분류분석(Classification Analysis)

- 데이터가 어떤 그룹에 속하는지 예측하는데 사용
- K-최근접 이웃(K-Nearest Neighborhood), 의사결정 나무(Decision Tree), 베이지안 정리(Bayesian Theorem)를 이용한 분류, 인공신경망(Artificial Neural Network), 지지벡터기계(Support Vector Machine) 등을 세부 기법으로 사용

### 회귀분석(Regression Analysis)

- 원인이 되는 변수(독립 변수)와 결과로 간주되는 변수(종속 변수) 관계가 성립될 때, 하나 또는 여러 개의 독립변수와 종속변수 간의 관계를 분석하는 통계 기법
- 입력 데이터에서 변수간의 관계를 가장 잘 설명할 수 있는 y = a + bx 형태의 선형방정식을 찾아내고, 이를 통해 주어진 독립변수에 대해 종속 변수의 값을 예측
- 독립변수의 개수에 따라 단순선형회귀와 다중회귀모형으로 나뉨(다중회귀모형이 많이 쓰인다)

### 군집분석Clustering Analysis)

- 특성에 따라 객체를 여러 개의 배타적인 집단으로 나누는 것

- 데이터의 자발적인 군집화를 유도

- 군집으로 나뉜 뒤 각 군집의 특성과 군집간의 차이 등에 대해 분석

- 계층적 군집 방법과 비계층적 군집 방법이 있음

  - 계층적 군집 방법(Hierarchical Clustering)

    n개의 군집으로 시작해 점차 군집의 개수를 줄여나가는 방법이다.

  - 비계층적 군집 방법(Nonhierarchical Clustering)

    - n개의 개체를 g개의 군집으로 나눌 수 있는 모든 방법을 점검해 최적화한 군집을 형성

    - K-Means 군집화가 대표적인 예

      - 장점

        데이터의 내부구조에 대한 사전정보 없이 의미 있는 구조를 찾을 수 있으며, 객체간의 거리를 데이터 형태에 맞게 정의하면 모든 형태의 데이터에 대해 적용 가능

      - 단점

        가중치와 거리 정의가 어려우며 초기 군집수를 결정하기 어려움

### 연관분석(Association Analysis)

- 흔히 말하는 장바구니 분석, 서열 분석

- 빈발 항목 집합과 연관 규칙 도출에 중점

## 관련 도구

### 머하웃

- 확장성 있는 기계학습 라이브러리

- 기본적으로 자바만 지원하나 MLlib와 혼용하면 스칼라와 파이썬까지 함께 쓸 수 있다.

- 머하웃 소프트웨어 스택은 아래와 같다.

  ![](http://www.dbguide.net/publishing/img/dbguide/bigdata_technology/412_bigdata_01.png)

- 주요 알고리즘

  - 분류
  - 군집화
  - 추천 시스템
  - 장바구니 분석

- [공식 사이트 및 지원 알고리즘 확인](https://mahout.apache.org/)

### Spark MLlib

- 현재 제일 많이 쓰임

- 하나에 귀속되지 않는 범용성 목적의 빅데이터 플랫폼

- 데이터를 메모리에 올리는 형태가 달라 머하웃과 같은 타 라이브러리와 비교 시 더 빠른 결과를 얻어낼 수 있어 많이 사용

- 2014년도 이후 머하웃에 추가된 알고리즘은 스파크 기반

- 위에서 언급한대로 자바, 스칼라, 파이썬을 지원하나 메인 언어는 스칼라

- 그래프 알고리즘

  ![](http://www.dbguide.net/publishing/img/dbguide/bigdata_technology/413_bigdata_02.png)

  - [공식 사이트 및 활용법](https://spark.apache.org/mllib/)

