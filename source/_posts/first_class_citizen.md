---
title: 일급 시민 혹은 일급 객체 (First Class Citizen)
date: 2018-11-12 23:49:16
categories:
  - development
---

파이썬 데코레이터를 공부하려다가, 클로져를 보게되고, 결국 보게된 "일급 시민" 개념 정리.

## 최초 언급

프로그래밍 언어에서의 일급 시민(First class citizen. 이하 일급 객체)과 이등 시민(Second class citizen) 개념은 1960년대에 영국의 컴퓨터 과학자 [크리스토퍼 스트래치](https://en.wikipedia.org/wiki/Christopher_Strachey)가 최초로 소개하였다.  

사실 스트래치 씨가 엄밀하게 정의하지는 않았다. 프로그래밍 언어 ALGOL의 Real Number와 Procedure를 비교하면서, Real Number는 변수에서 대입될 수 있고 
Procedure의 Parameter도 될 수 있는데, Procedure는 반환값(Return value)로 전달되거나 연산식 내에서 사용될 수 없다고 **이등 시민(Second class citizen)** 이라고 표현했다. ("이등 시민"이라는 단어를 거리낌없이 사용한 것이 놀라워서 찾아보니, 크리스토퍼 스트래치는 [스트래치 가문](https://en.wikipedia.org/wiki/Strachey)의 일원이었다. 당시 귀족들은 이런 단어를 일반적으로 사용했을까?)

## 필수 조건

영국의 과학자 [Robin Popplestone](https://en.wikipedia.org/wiki/Robin_Popplestone)은 일급 객체의 조건을 아래와 같이 정리했다.

1. 함수의 인자로 전달될 수 있다. (*All items can be the actual parameters of functions*)
2. 함수의 반환값(return value)이 될 수 있다. (*All items can be returned as results of functions*)
3. 변수에 할당될 수 있다. (*All items can be the subject of assignment statements*)
   - 초의역.
4. 동등함을 판단할 수 있다. (고유한 구별이 가능하다.) (*All items can be tested for equality.*)

## 3등 시민?

[Raphael Finkel](https://en.wikipedia.org/wiki/Raphael_Finkel)라는 과학자가 2등시민과 3등시민의 정의를 내렸지만, 널리 받아들여지지는 않았다고 한다.

## 일급 함수 (First Class Function)

프로그래밍 언어가 함수를 일급 시민으로 취급하면, 일급 함수라고 부른다. 대표적인 예로 Python이 그러하다. First class function이기 때문에 클로져, 데코레이터의 구현이 가능하다.