---
title: Python Logging - Formatter 클래스 (1/3)
date: 2018-12-12
categories:
  - development
---

`Formatter` 클래스 인스턴스는 `LogRecord`를 텍스트로 변환할 때 사용된다. (사람 혹은 외부 시스템이 해석할 수 있는 문자열)

## Format String

LogRecord 인스턴스의 속성들로 포맷 문자열(format string)을 만든다. 

기본값 및 종류는 아래와 같다.

```python
BASIC_FORMAT = "%(levelname)s:%(name)s:%(message)s"

_STYLES = {
    '%': (PercentStyle, BASIC_FORMAT),
    '{': (StrFormatStyle, '{levelname}:{name}:{message}'),
    '$': (StringTemplateStyle, '${levelname}:${name}:${message}'),
}
```

### 주요 LogRecord 속성

자주 쓰이는 LogRecord 속성들은 아래와 같다. 커스텀 포맷 문자열을 만들어서 쓴다면, 참고할 것!

```
%(name)s            logger의 이름 (logging 채널)
%(levelno)s         로깅 레벨의 숫자값 (10, 20, 30, 40, 50)
%(levelname)s       로깅 레벨의 이름 ("DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL")
%(pathname)s        로깅이 호출된 소스파일의 경로 (full path)
%(filename)s        파일이름 (위 경로에서 파일이름 부분만, 확장자 포함)
%(module)s          모듈명 (파일이름에서 확장자 제외)
%(lineno)d          로깅이 호출된 코드 라인 숫자
%(funcName)s        함수 명
%(created)f         LogRecord가 생성된 시간 (반환값 : time.time())
%(asctime)s         created의 문자열 표현
%(msecs)d           created의 Millisecond 시간
%(thread)d          Thread ID
%(threadName)s      Thread 이름
%(process)d         Process ID
%(message)s         `record.getMessage()`의 결과
```
