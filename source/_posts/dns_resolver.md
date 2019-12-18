---
title: DNS Resolver
date: 2019-02-04
categories:
  - development
---

## 역할

1. Client의 (Domain Name Resolving) 요청을 받아
2. 여러 단계의 네임서버에 접근하여 (재귀 요청)
3. Domain Name에 해당하는 IP를 찾아서 Client에 알려주는 역할

## Stub Resolver

위의 역할을 Client Host에서 모두 수행하기는 빡셈

그래서 위의 역할(2)을 DNS 서버에서 대신하고 Client Host의 Resolver에서는 요청-응답의 Interface 역할만 함
(Local DNS Server)

이를 Stub Resolver라고 함
