---
title: 순수 함수 (Pure Function)
date: 2018-11-17 23:49:16
categories:
  - development
---

컴퓨터 프로그래밍 분야에서, 순수 함수(Pure Function)은 아래의 2가지 속성을 가진 함수이다.

1. 같은 인자에 대해 같은 반환 값을 가진다.  
  (로컬 변수, 비-로컬 변수(전역 변수 등), 변동 가능한 참조 인자,  에 의한 변화 X
  , no variation with local static variables, non-local variables, mutable reference arguments or input streams from I/O devices).
2. 부수 효과(Side Effect)가 없다.
Its evaluation has no side effects (no mutation of local static variables, non-local variables, mutable reference arguments or I/O streams).
Thus a pure function is a computational analogue of a mathematical function. Some authors, particularly from the imperative language community, use the term "pure" for all functions that just have the above property 2[3][4] (discussed below).


## Link
- [Wikipedia](https://en.wikipedia.org/wiki/Pure_function)