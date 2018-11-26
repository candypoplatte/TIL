---
title: 20181122 AWS 서울 리젼 DNS 장애 내용
date: 2018-11-26
categories:
  - development
---

지난 목요일 오전에 AWS 서울 리전에서 발생했던 DNS 이슈에 대한 공식 요약문이 올라옴

## 1줄 요약

설정 값을 잘못 바꿔서 생긴 *내부* DNS Resolver 서버 이슈

## 3줄 요약

1. AWS에 EC2를 위한 private DNS Resolver 서버 그룹이 있다.

2. 이 서버 그룹 내에 존재하는 호스트의 `최소 정상 상태 갯수` 설정값이 잘못 수정되어서 정상 호스트의 갯수가 줄어들어 버림

3. DNS 서버의 갯수가 줄어들면서, EC2 인스턴스들의 도메인 네임 해석 요청이 실패하기 시작했다. (트래픽) 

## Links

- [요약문 Link](https://aws.amazon.com/message/74876/)