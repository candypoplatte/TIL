---
title: Python Logging - 로깅 레벨, getLevelName/addLevelName
date: 2018-11-27
categories:
  - development
---

요즘 파이썬의 빌트인 패키지 중 로깅(Logging) 패키지를 열심히 보고 있다.

## Logging Package

- 파이썬 빌트인 패키지
- PEP 282를 기반으로 함
- 주요 개발자 : Vinay Sajip

## Logging Level

로그의 중요도를 비교할 수 있는 척도. 정수로된 상수이다. Level이 너무 많으면 Level을 선택하기 힘들어져 혼란스러울 수 있다.

파이썬 로깅 패키지에 기본 설정된 Logging Level 상수는 아래와 같다.

```python
CRITICAL = 50
FATAL = CRITICAL
ERROR = 40
WARNING = 30
WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0
```

그리고 level(숫자) <-> name(문자열) 양방향 매핑을 위한 두 개의 Dictionary가 있다.
 
```python
_levelToName = {
    CRITICAL: 'CRITICAL',
    ERROR: 'ERROR',
    WARNING: 'WARNING',
    INFO: 'INFO',
    DEBUG: 'DEBUG',
    NOTSET: 'NOTSET',
}
_nameToLevel = {
    'CRITICAL': CRITICAL,
    'FATAL': FATAL,
    'ERROR': ERROR,
    'WARN': WARNING,
    'WARNING': WARNING,
    'INFO': INFO,
    'DEBUG': DEBUG,
    'NOTSET': NOTSET,
}
```

## getLevelName 함수

> getLevelName(level)

Logging Level 혹은 Logging Level Name을 반환하는 함수.

1. 미리 정의된 Logging Level 이면, Logging Level Name 반환
2. 미리 정의된 Logging Level Name 이면, Loggin Level 반환
3. "Level <level>" 반환

깔끔하게 Level을 넣으면 Level Name을 반환한다거나 하지 않고, Level과 Level Name을 다 받아주는 함수다!

```python
def getLevelName(level):
    # See Issues #22386, #27937 and #29220 for why it's this way
    result = _levelToName.get(level)
    if result is not None:
        return result
    result = _nameToLevel.get(level)
    if result is not None:
        return result
    return "Level %s" % level

```

[이슈 #22386](https://bugs.python.org/issue22386), [이슈 #27937](https://bugs.python.org/issue27937), [이슈 #29220](https://bugs.python.org/issue29220)를 살펴보니 처음에는 아래와 같이 아주 간단했었다. 여러 이슈를 거치면서 아주 관대한 함수가 되어버림!

```python
def getLevelName(level):
    return _levelToName.get(level, ("Level %s" % level))
```

그리고 camelCase로 되어있다...!

## addLevelName 함수

> addLevelName(level, levelName)

level과 대응되는 levelName을 추가하는 함수.

`_levelToName`과 `_nameToLevel` Dictionary에 매핑 정보를 추가한다. 추가하기 전에 shared data에 대한 Lock을 획득하고, 추가 후 해제한다.

```python
def addLevelName(level, levelName):
    _acquireLock()
    try:
        _levelToName[level] = levelName
        _nameToLevel[levelName] = level
    finally:
        _releaseLock()
```

## Links

- [PEP 282](https://www.python.org/dev/peps/pep-0282/)
- [이슈 #22386](https://bugs.python.org/issue22386)
- [이슈 #27937](https://bugs.python.org/issue27937)
- [이슈 #29220](https://bugs.python.org/issue29220)
- [Python Logging Package (3.7)](https://github.com/python/cpython/blob/3.7/Lib/logging/__init__.py)