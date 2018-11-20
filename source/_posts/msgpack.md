---
title: MessagePack
date: 2018-11-20
categories:
  - development
---

Binary Serialization Format. 데이터를 바이너리 형태로 직렬화한다. JSON보다 용량이 훨씬 작아 빠른 통신에 유용하다. 

Redis와 Fluentd의 메시지 포맷으로 사용된다.

## JSON VS MessagePack

<img src="/TIL/images/msgpack.png" width="50%">

## Test in Python

### 설치

```bash
pip install msgpack
```

### 실행

```python
>>> import msgpack
>>> msgpack.packb({'number': 3})
b'\x81\xa6number\x03'
>>> msgpack.unpackb(_, raw=False)
{'number': 3}
```

## Links
- [홈페이지](https://msgpack.org/)
