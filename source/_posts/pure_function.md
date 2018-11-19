---
title: 순수 함수 (Pure Function), 부수 효과 (Side Effect)
date: 2018-11-17
categories:
  - development
---

컴퓨터 프로그래밍 분야에서, 순수 함수(Pure Function)은 아래의 2가지 속성을 가진 함수이다.

1. 같은 인자에 대해 같은 반환 값을 가진다.  
    - 비-로컬 변수, 로컬 정적 변수, 참조에 의해 전달된 인수, I/O에 의한 변화가 없어야 함
2. 부수 효과(Side Effect)가 없다.
    - 비-로컬 변수 수정, 로컬 정적 변수 수정, 참조에 의해 전달된 인수 수정, I/O 수행을 하지 않아야 함

말 그대로 외부 환경에 영향을 주지도 받지도 않는 함수이다.

## 부수 효과 (Side Effect)

> 프로그래밍 분야에서는 처음보는 단어!

컴퓨터 과학 분야에서, 함수가 Local 환경 밖의 상태를 변경할 때 그 함수는 "부수 효과"가 있다고 표현한다. (값을 반환하는 것 외에 외부 세계와 상호작용을 할 때)

위에서 언급한 대로, 비-로컬 변수 수정, 로컬 정적 변수 수정, 참조에 의해 전달된 인수 수정, I/O 수행을 하지 않아야 한다.

부수 효과가 있는 함수는 작동 순서가 중요하며, 디버깅하기 위해서는 Context와 실행 기록에 대한 지식이 필요하다.

## Link

- [순수함수@Wikipedia](https://en.wikipedia.org/wiki/Pure_function)
- [부수효과@Wikipedia](https://en.wikipedia.org/wiki/Side_effect_\(computer_science\))