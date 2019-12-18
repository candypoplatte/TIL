---
title: 강의 - DevOps의 이해 및 Docker Hands-on (2/3)
date: 2019-08-06
categories:
  - development
---

인프런의 DevOps & Docker 강의의 2/3 부분. Docker에 대해 간략히 설명한다.

## Docker의 특징

### Linux Container(LXC) 기반

- Virtual Machine과는 다름
- Host OS의 커널을 공유함
- 부팅 과정이 없다
- 도커 이미지를 메모리에 올리면 -> 부팅이 된 것이나 마찬가지

### cgroup : Control Group

- 프로세스 격리 방식
- 나중에 찾아보자

### Docker Hub

- LXC와는 달리 만들어놓은 이미지를 공유 -> Boom

### aufs

- 적층형 이미지
- Base +> 소스a +> 소스b
- 버전 변경 시 차이나는 부분만 있으면 됨. 이미 있는 부분은 받지 않음
- 저장소 관리 효율 & 네트워크 절약
- 나중에 찾아보자

### Device-mapper

- 볼륨 관리 기술
- Ubuntu 특화
- 나중에 찾아보자

### 버전 표기 방식

- 연도.월
- ex) 18.03

## 마무리

**Docker는 LXC를 쓰기 편하게 만든 것!**

## Docker 실습 환경 구축

### Vagrant vs Docker-machine

- docker-machine : Boot2Docker라는 이미지만 사용 가능
