---
title: 서브 도메인(하위 도메인)이 다른 사이트를 하나의 GTM으로 추적하기
date: 2019-12-18
categories:
  - development
---

Cross Domain 추적을 위한 설정이 아님. Sub Domain이 다른 사이트간 추적을 위한 설정임

- 예시) `good.candypoplatte.com, better.candypoplatte.com, best.candypoplatte.com`

## 방법

### 1. [GTM] 자동 링크 도메인 변수 생성

- 이름 : `Auto Link Domains` (이름은 자유롭게)
- 유형 : `Constant`
- 값 : 추적을 원하는 사이트 리스트 (Comma로 구분)

### 2. [GTM] GA Setting 변수 설정 - 자동 링크 도메인 추가

- GA Setting 변수 → More Settings → Cross Domain Tracking → Auto Link Domains 설정 → `Auto Link Domains` 변수 입력

### 3. [GTM] GA Setting 변수 설정 - 기타 설정

- 쿠키 도메인 설정 값 : `auto`
- 필드
  - `cookieDomain` : `auto`
  - `allowLinker` : `true`

### 4. [GA] 추천 제외 목록 설정

도메인 변경 시 새 GA 세션을 생성하지 않기 위해 설정함

- 메뉴 : 관리 → 추적 정보 → 추천 제외 목록
- 설정 값 : 서비스 도메인 (ex. `candypoplatte.com`)

## TMI

- Facebook Pixel, Hotjar 설정 시 Trigger 조건에 Host Domain Name을 반드시 명시해야 함
- 참고한 문서 : [https://www.bounteous.com/insights/2015/06/16/cross-domain-tracking-google-tag-manager/](https://www.bounteous.com/insights/2015/06/16/cross-domain-tracking-google-tag-manager/?ns=l)