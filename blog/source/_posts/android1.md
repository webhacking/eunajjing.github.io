---
title: 안드로이드 스튜디오를 이용해 안드로이드 앱 만들기
date: 2018-12-05 09:06:18
tags:
categories:
- 개발공부
- 안드로이드
---

## 설치

1. Java JDK 설치 및 환경변수 설정
2. 안드로이드 스튜디오 설치

## Hello, World!

- MainActivity
  - 프로그램을 제일 먼저 실행시키는 부분
- R.class
  - 안드로이드는 모든 것에 다 복잡한 숫자의 id가 붙어있다.
  - id를 사용자가 직접 타이핑해서 사용하는 건 힘들기 때문에 R 클래스 안 아이디 사용에 관한 static 변수/메서드를 구현해 놓아 사용자가 보다 쉬운 방식으로 요소를 이용할 수 있도록 했다.
- 자동 import 단축키 **alt + Enter**
- 상속 메서드들 보기 단축키  **ctrl + o**

## 리스트 뷰 꾸미기

<iframe width="760" height="453" src="https://www.youtube.com/embed/oj6DI3PvAr0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> ## 참고하고 있는 강의
>
> 기존에 처음 html로 페이지를 만들 적에 이 분이 만든 강의를 처음 들었었는데, 당시에 많은 도움을 받았던 기억이 나 이번에도 이 분 강의를 듣는 중!

### 에러 트러블슈팅

- 처음에는 강의와 똑같이 했는데도 실행이 안됐다. 강의가 만들어진지 좀 됐기 때문에, 나는 당연히 조금 최신 버전의 os와 에뮬레이터를 선택해 실행했는데... 이게 바로 에러의 서막이었다...

- 처음에는 맥과 JDK가 충돌나서 그런가 했다. 왜냐하면 지금 안드로이드 스튜디오를 맥 OS로 다운로드 받을 때, JDK 일부 버전에서 에러가 날 수도 있다는 경고가 뜨기 때문이다.

  > 관련하여 맥 OS에서 안드로이드 스튜디오로 앱 만드시는 분께 여쭤봤는데 본인은 잘만 돌아간다며 걱정 말라고....

- avd에 관한 에러였는데, 찾아보니까 Emulated Performance 부분의 Graphics을 auto가 아니라 Software - GLES 2.0으로 바꾸면 된다고 했다.

- 나도 바꾸려고 했지만 바꿀 수 없게 블록 잼....

- 언제나 그렇듯 구글링하며 흐느끼다가 마음의 고향 [스택옾](https://stackoverflow.com/questions/44328225/cant-change-emulated-performance-of-avd-in-android-studio)에 갔고 해결책을 얻었다. 고마워여 스택옾...!

- 제일 위에 채택 받은 답변을 보면, 구글 플레이스토어를 지원하는 기기의 경우에 바꿀 수 없게 막혀있다고 했다.
- 즉 다른 기기를 선택하면 된다고...
- 그래서 해결...

