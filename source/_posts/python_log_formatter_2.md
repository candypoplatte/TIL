---
title: Python Logging - Formatter 클래스 (2/3)
date: 2018-12-22
categories:
  - development
---

## `__init__` Method

파라미터로는 포맷 문자열(`fmt`), 날짜 포맷 문자열(`datefmt`), 스타일 문자열(`style`)을 받음

각 파라미터는 기본값이 있음

- style 파라미터의 기본값은 `%` 이고, 해당하는 Style Class는 `PercentStyle`임
- fmt 파라미터가 생략된 경우, Style Class의 Class 변수 중 `default_format`의 값으로 지정됨
- datefmt 파라미터를 생략할 경우, `formatTime` Method에서 ISO8601(혹은 RFC 3339) 스타일의 포맷으로 지정됨

```python
def __init__(self, fmt=None, datefmt=None, style='%'):
    if style not in _STYLES:
        raise ValueError('Style must be one of: %s' % ','.join(
                          _STYLES.keys()))
    self._style = _STYLES[style][0](fmt)
    self._fmt = self._style._fmt
    self.datefmt = datefmt
```

## `formatTime` Method

LogRecord의 생성 시간을 포매팅된 문자열로 반환하는 메서드. formatter의 `format()` 메서드에서 호출됨. 

- datefmt이 있으면, time.strftime()을 호출하여 해당 포맷으로 포매팅
- datefmt이 없으면, ISO8601(혹은 RFC 3339) 스타일의 포맷으로 포매팅 (`%Y-%m-%d %H:%M:%S,%03d`)

- `time.localtime()`가 사용됨. GMT를 원하는 경우, `converter` attribute를 `time.gmtime()` 으로 변경하면 됨.

```python
# converter의 위치는 여기가 아님. 설명을 위해 불러옴
converter = time.localtime

default_time_format = '%Y-%m-%d %H:%M:%S'
default_msec_format = '%s,%03d'

def formatTime(self, record, datefmt=None):
    ct = self.converter(record.created)
    if datefmt:
        s = time.strftime(datefmt, ct)
    else:
        t = time.strftime(self.default_time_format, ct)
        s = self.default_msec_format % (t, record.msecs)
    return s
```

## `formatException` Method

exception 정보를 문자열로 포매팅하여 반환하는 메서드

`traceback.print_exception()`를 통해 Traceback 정보가 기록됨.

`print_exception` 함수가 `file` 파라미터에 (없으면 `sys.stderr`) Traceback 정보를 프린트하도록 되어 있어, `io.StringIO()`를 활용하여 문자열에 기록 후 `getvalue()`로 문자열 값을 반환한다. (단, 마지막이 줄바꿈 문자열이면 삭제함)

```python
def formatException(self, ei):
    sio = io.StringIO()
    tb = ei[2]
    traceback.print_exception(ei[0], ei[1], tb, None, sio)
    s = sio.getvalue()
    sio.close()
    if s[-1:] == "\n":
        s = s[:-1]
    return s
```

## `usesTime` Method

포맷 문자열에 생성 시간이 포함되어 있는지 확인하는 메서드

보통 스타일 Class에 정의되어있음. (`PercentStyle` 클래스의 경우, `%(asctime)` 문자열이 포함되어있는지 확인함.)

```python
def usesTime(self):
    return self._style.usesTime()
```

(`PercentStyle` 클래스 예시)

```python
class PercentStyle(object):
    asctime_search = '%(asctime)'

    def usesTime(self):
        return self._fmt.find(self.asctime_search) >= 0
```