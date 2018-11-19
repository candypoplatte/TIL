---
title: getLogger 함수는 오직 한 인스턴스만 반환
date: 2018-11-12 23:49:16
categories:
  - development
---

파이썬 로깅(Logging)을 공부하려고 [파이썬 공식 로깅 튜토리얼 문서](https://docs.python.org/ko/3/howto/logging.html#logging-basic-tutorial)를 보다가 `logging` 모듈의 `getLogger` 함수의 특징을 정리해 봄


## getLogger 함수의 특징

> logging.getLogger(name=None)

로거(Logger)를 반환하는 함수.

입력된 name의 로거를 반환하며, name이 None인 경우에는 root 로거를 반환한다. name은 보통 "a", "a.b.c"와 같이 점으로 구분된 계층적 구조의 문자열이다.

같은 name으로 함수를 여러 번 호출해도 같은 로거 인스턴스를 반환한다.

## Code

아래 코드를 보면 loggerDict에 name이 이미 저장되어 있으면 저장되어있는 객체를 반환하고, 아니면 로거 객체를 생성하여 반환하는 모습을 볼 수 있다.

```python
def getLogger(self, name):
    rv = None
    if not isinstance(name, str):
        raise TypeError('A logger name must be a string')
    _acquireLock()
    try:
        if name in self.loggerDict:
            rv = self.loggerDict[name]
            if isinstance(rv, PlaceHolder):
                ph = rv
                rv = (self.loggerClass or _loggerClass)(name)
                rv.manager = self
                self.loggerDict[name] = rv
                self._fixupChildren(ph, rv)
                self._fixupParents(rv)
        else:
            rv = (self.loggerClass or _loggerClass)(name)
            rv.manager = self
            self.loggerDict[name] = rv
            self._fixupParents(rv)
    finally:
        _releaseLock()
    return rv
```

## Links
- [python logging 모듈의 getLogger 함수](https://github.com/python/cpython/blob/3.7/Lib/logging/__init__.py#L1221)
- [파이썬 공식 로깅 튜토리얼 문서](https://docs.python.org/ko/3/howto/logging.html#logging-basic-tutorial)
