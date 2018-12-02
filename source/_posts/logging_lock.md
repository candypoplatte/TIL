---
title: Python Logging 파헤치기 - Lock (_acquireLock, _releaseLock)
date: 2018-11-29
categories:
  - development
---

[파이썬 로깅 패키지](https://github.com/python/cpython/blob/3.7/Lib/logging/__init__.py)에서 가장 많이 보이는 패턴이 Lock을 얻고 해제하는 부분이다.

보통 아래와 같이 사용한다.  
Lock을 얻고, try문에서 원하는 코드를 실행하고, 반드시 실행되는 finally 문에서 Lock을 해제한다.

```python
_acquireLock()
try:
    코드
finally:
    _releaseLock()
```

## 코드 살펴보기

```python
if threading:
    _lock = threading.RLock()
else:
    _lock = None


def _acquireLock():
    if _lock:
        _lock.acquire()

def _releaseLock():
    if _lock:
        _lock.release()
```

## RLock 사용

RLock에 대한 내용은 [이전에 정리한 쓰레드, 락 TIL 참고](/TIL/2018/11/29/threading_rlock/)

### Why RLock?

왜 RLock을 사용하는지는 주석에 기록되어있다!  

[fileConfig 함수](https://github.com/python/cpython/blob/master/Lib/logging/config.py#L51)가 실행되면 handler를 재등록 하는데 이 때 `fileConfig`에서도 Lock을 얻고, shared data인 `_handlers` 딕셔너리에 handler를 등록하기 위해 호출되는 `Logger.addHandler`에서도 Lock을 얻는다. 이 때 쓰레드가 Block되지 않기 위해 RLock을 사용한다.

## _acquireLock(), _releaseLock()

Lock을 얻고, 해제하는 부분을 함수로 래핑하여 사용한다.

다른 모듈에서 사용될 때는 `logging._acquireLock()` 형태로 사용하여 logging 모듈에서 선언된 _lock 객체를 그대로 사용할 수 있다.
