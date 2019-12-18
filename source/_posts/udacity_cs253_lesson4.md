---
title: Udacity CS253 Lesson4 "User Accounts and Security"
date: 2019-06-08
---

쿠키에 대해 공부하기 위한 영상으로 Udacity의 CS253-Lesson4를 선택함

## 강의 요약 #1 : 쿠키에 대하여

### 쿠키란?

- 쿠키 : 브라우저 내 작은 데이터 저장소 (for 웹사이트)
- "이름=값" 형태로 저장됨 (<cookie-name>=<cookie-value>)
  - ex. user_id = 12345
- 보통 브라우저에 로그인(인증)한 결과를 저장
- 브라우저가 저장하고 있는 문서 파일이라고 생각하면 됨
- 브라우저마다 제한이 있음
  - 20쿠키 per 웹사이트
  - 4킬로바이트 이하
  - only for 1 website
  - [https://stackoverflow.com/questions/2543851/chrome-cookie-size-limit](https://stackoverflow.com/questions/2543851/chrome-cookie-size-limit)
  - [http://browsercookielimits.squawky.net/](http://browsercookielimits.squawky.net/)
- 사이즈가 작은, 웹사이트에 국한되는 정보 저장하기에 적합

### HTTP Header

- 쿠키의 내용을 전달하거나, 내용 수정 명령은 HTTP Header에 담겨서 전달된다
- HTTP 응답 헤더 "Set-Cookie"
  - 서버 -> 클라이언트 
  - 쿠키 내용 수정 명령
  - 예) Set-Cookie : user_id=12345
  - 두개면
    - Set-Cookie : userid=12345
    - Set-Cookie : asdf=12345
    - HTTP 헤더는 name이 유니크할필요는없음
- HTTP 요청 헤더 "cookie"
  - 클라이언트 -> 서버
  - cookie: user_id12345
- 크롬브라우저에서 테스트 해봄

### Cookie Domains / Path

- Domain 디렉티브
  - 쿠키는 Domain에 국한됨
  - 서브도메인은 포함됨 (www.reddit.com으로 지정했으면 kkk.www.reddit.com은 가능. bar.reddit.com은 불가능)
- Path 디렉티브
  - 보통 "/" (해당 domain의 모든경로)

### 크롬에서 쿠키 설정 확인하기

- chrome://settings
- 아는만큼 보인다!

### google analytics가 사용자의 인터넷 접속 자취를 파악하는 방법

- 작은 이미지를 심어놓음 (ex. img.google.com/1q2w3e4r5t)
- google.com에 쿠키를 보내도록 되어있음

### Cookie 만료

- Expires가 있으면 해당 일시에 만료됨
  - ex. Tue 1 Jan.....
- Expires가 없으면 브라우저 닫으면 사라짐
  - 이걸 session cookie라 부른다

### 쿠키 조작

- 자바스크립트로 쿠키를 조작할 수 있다!
- HTTPONLY, SECURE 설정으로 막을 수는 있음

## 강의 요약 #2 : HASH에 대하여

### 해싱

- legitimacy : 정통성
- legitimacy를 체크하기 위한 용도임
- H : hashing function
- H(x) → y
  - x : data
  - y : 고정된 사이즈의 bit string
- y로 x 찾기 불가능
- x에 대해 유니크한 y생성
- 더 궁금하면 CS387 참고

### 해시 알고리즘

- 절대 자체 알고리즘 쓰지마라
- 목록 (내려갈수록 느림. 내려갈수록 보안 good)
  - CRC32
    - 체크썸용. 빠름
    - 보안에 취약
    - collision : x1과 x2가 같은 Y를 가지는거
    - 이 알고리즘은 collision을 가짐
  - MD5
    - 가장 유명. 빠름(CRC보다는아님)
    - 보안에 취약 : 더 이상 안전하지 않음, y에서 x찾기가 쉬움 (강의에서는 "최근 몇년전부터"라는 말을 함)
  - sha1
  - sha256 :

### Python Library

- hashlib

### Cookie에 해시값 추가하기

- 쿠키 값 뒤에 해시를 붙이기
  - 예) set-cookie: visit=5, [hash]
- 이거 JWT의 SIGNATURE와 같군
  - 해시의 용도 대로 잘 쓰고 있군

### cookie hasing

- HMAC : Hash based message Authentication Code
- HMAC(secret, key, H)
- HMAC에 H가 input으로 들어간다!

### 서버에서 Cookie SET & Validation

- JWT 와 같군....
- JWT는 obaque가 아닌 토큰! <- 이게 다군. 나머지는 Session ID에 JWT를 쓰냐 정도로 갈리는 문제

## 패스워드 해싱하라

- 당연한 말씀!

### sad story time

- 레딧 만든 사람이 옛날 얘기 해 줌

### Rainbow Tables

- 비밀번호가 해싱되었다고 무조건 안전한건 아님
- [https://ko.wikipedia.org/wiki/레인보_테이블](https://ko.wikipedia.org/wiki/%EB%A0%88%EC%9D%B8%EB%B3%B4_%ED%85%8C%EC%9D%B4%EB%B8%94)
- 레인보우테이블을 확보하면 ㅈ 됨
- salt : 랜덤스트링
  - H(pw+salt)
- [http://kestas.kuliukas.com/RainbowTables/](http://kestas.kuliukas.com/RainbowTables/)

### Bcrypt

- 많은 해싱 함수의 문제점 : 빠르게 작동하도록 설계됨
  - 오오 이게 단점이구나 (공격자 입장)
- Bcrypt는 패스워드 저장을 목적으로 설계된 알고리즘
- work factor라는 인자로 얼마만큼의 처리 과정을 수행할지 결정한다고함
  - 오오 일부러 늦어지게!!!
- [https://d2.naver.com/helloworld/318732](https://d2.naver.com/helloworld/318732)
