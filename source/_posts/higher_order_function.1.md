---
title: Redis SET 커맨드의 NX 옵션
date: 2018-12-21
categories:
  - development
---

Redis 사용 시, key에 data를 저장할 때 SET 커맨드를 사용한다.

NX 옵션 : 키가 존재하지 않을 때만 SET이 가능하게 한다.

```bash
127.0.0.1:6379> SET k1 v1
OK
127.0.0.1:6379> GET k1
"v1"
127.0.0.1:6379> SET k1 v2
OK
127.0.0.1:6379> GET k1
"v2"
127.0.0.1:6379> SET k1 v3 NX
(nil)
127.0.0.1:6379> GET k1
"v2"
127.0.0.1:6379> DEL k1
(integer) 1
127.0.0.1:6379> SET k1 v3 NX
OK
127.0.0.1:6379> GET k1
"v3"
```