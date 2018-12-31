---
title: Python Logging - LogRecord 클래스 팩토리 함수
date: 2018-12-06
categories:
  - development
---

logging 패키지에는 어떤 Log Record 클래스를 사용할 지 정하기 위한 팩토리 함수들이 총 3개 있다.

우선 Default는 원래 `LogRecord` 클래스로 지정되어있다.

```python
_logRecordFactory = LogRecord
```

## setLogRecordFactory

로그 레코드를 초기화할 때 사용되는 로그 레코드 팩토리를 정하는 함수. 글로벌 변수 `_logRecordFactory`를 입력받은 `factory`로 대치한다.

```python
def setLogRecordFactory(factory):
    global _logRecordFactory
    _logRecordFactory = factory
```

## getLogRecordFactory

글로벌 변수 `_logRecordFactory`를 반환하는 함수

```python
def getLogRecordFactory():
    return _logRecordFactory
```

## makeLogRecord

Dictionary를 받아 로그 레코드 객체를 생성하여 반환하는 함수

```python
def makeLogRecord(dict):
    rv = _logRecordFactory(None, None, "", 0, "", (), None, None)
    rv.__dict__.update(dict)
    return rv
```