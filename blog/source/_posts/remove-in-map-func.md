---
title: map((item, index)=> {console.log('index 그만하기')})
date: 2019-05-19 21:08:18
tags:
categories:
- 개발공부
- React
---

# `map((item, index)=> {console.log('index 그만하기')})`

## `index` 사용을 지양하는 이유

> 보고 있는 [리액트 책](https://book.naver.com/bookdb/book_detail.nhn?bid=13799583)에서 너무 당연하게 `map()`으로 구현한 컴포넌트의 `key`값으로 `index`를 사용해서 나 또한 그런 식으로 작성 중에 있었는데,

- [노마드 코더 강의](https://academy.nomadcoders.co/courses/216871/lectures/3368672)에 따르면 `index`를 사용하는 게 보다 느려진다고, 만약 고유한 값이 이미 있다면 그걸 쓰는 게 좋다고 했다.
  - 실제로 해당 강의에선 처음엔 `index`로 `key`를 구현했다가, api를 호출하며 해당 pk를 key로 쓰는 방식으로 바꾼다
- 오늘 오프라인 리액트 강의를 듣다가, 강의 하시는 분이 `map()`으로 컴포넌트를 배열화 해서 뿌려주는 경우, **배열 안에서 값이 달라질 여지가 있기 때문에** 고유한 값으로 만드는 게 낫다고 하셨다.