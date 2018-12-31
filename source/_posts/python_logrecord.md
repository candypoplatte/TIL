---
title: Python Logging - LogRecord 클래스
date: 2018-12-02
categories:
  - development
---

파이썬 로깅 패키지의 `init.py` 파일에서 제일 처음 선언되는 클래스는 `LogRecord`다.

## LogRecord?

LogRecord 인스턴스는 **로깅된 이벤트**를 나타낸다.

무언가가 **로깅될 때마다 생성**되며, 로깅되는 **이벤트와 관련된 모든 정보**를 담고있다. 전달되는 주요 정보는 `msg`와 `args`이며, `str(msg) % args`를 통해 Record의 message 필드를 생성하는데 사용된다. 또한 생성된 시기(created), 로깅 호출이 일어난 소스 라인, Exception 정보 또한 포함된다."*

## Methods

### `__init__`

LogRecord 클래스의 초기화 Method.

생성시간(created_time) `ct`를 가장 먼저 설정하고, 인스턴스 변수 `name`과 `msg`에 인자 값을 대입한다.

```python
def __init__(self, name, level, pathname, lineno,
              msg, args, exc_info, func=None, sinfo=None, **kwargs):
    ct = time.time()
    self.name = name
    self.msg = msg
```

바로 아래에는 전달된 argument가 단 하나의 Dictionary일 때 이 첫 번째 Dictionary Argument를 `args` 변수에 대입하는 코드가 있다. `logging.debug("a %(a)d b %(b)s", {'a':1, 'b':2})`처럼 작성 가능하게 하기 위함이다.

여기서 재밌는 부분은 Dictionary 임을 체크할 때 `isinstance(args[0], dict)`가 아니라 `isinstance(args[0], collections.Mapping)`로 체크하는 부분이다.

[Issue #21172](https://bugs.python.org/issue21172)를 확인하여 아래의 과정을 통해 작성된 것을 확인하였다.

1. 처음에는 `isinstance(args[0], dict)`로 작성이 됨
2. 어차피 String Formatting을 위한 Dictionary이니 정도만 체크하자는 (`hasattr(..., '__getitem__')`) Issue가 나옴
3. 논의 과정에서 String Formatting을 위한 Mapping을 할 때 Armument는 "single mapping object"여야 하기 때문에 `__getitem__`의 체크만으로는 부족하다는 의견도 나옴
4. 그래서 `dict` 보다는 완화된 `collections.Mapping` 클래스를 사용하게 됨

```python
    if (args and len(args) == 1 and isinstance(args[0], collections.Mapping)
        and args[0]):
        args = args[0]

    self.args = args
```

그 다음은 여러가지 변수의 초기화 과정이다. 로깅 레벨, 파일 이름, 모듈, 실행 정보, 스택, 라인, 함수, 생성시간 등 이벤트의 모든 정보가 다 있는 것을 볼 수 있다.

```python
    self.levelname = getLevelName(level)
    self.levelno = level
    self.pathname = pathname
    try:
        self.filename = os.path.basename(pathname)
        self.module = os.path.splitext(self.filename)[0]
    except (TypeError, ValueError, AttributeError):
        self.filename = pathname
        self.module = "Unknown module"
    self.exc_info = exc_info
    self.exc_text = None
    self.stack_info = sinfo
    self.lineno = lineno
    self.funcName = func
    self.created = ct
    self.msecs = (ct - int(ct)) * 1000
    self.relativeCreated = (self.created - _startTime) * 1000
```

쓰레드, 프로세스 정보도 포함된다. (ID, 이름)

```python
    if logThreads and threading:
        self.thread = threading.get_ident()
        self.threadName = threading.current_thread().name
    else: # pragma: no cover
        self.thread = None
        self.threadName = None
    if not logMultiprocessing: # pragma: no cover
        self.processName = None
    else:
        self.processName = 'MainProcess'
        mp = sys.modules.get('multiprocessing')
        if mp is not None:
            try:
                self.processName = mp.current_process().name
            except Exception: #pragma: no cover
                pass
    if logProcesses and hasattr(os, 'getpid'):
        self.process = os.getpid()
    else:
        self.process = None
```

### `__str__`

인스턴스의 문자열 표현 Method.

인스턴스 변수 `name`, `levelno`, `pathname`, `lineno`, `msg`를 하나의 문자열로 반환한다.



```python
def __str__(self):
    return '<LogRecord: %s, %s, %s, %s, "%s">'%(self.name, self.levelno,
        self.pathname, self.lineno, self.msg)

__repr__ = __str__
```

### `getMessage`

유저가 제공한 인수와, 메시지를 합친 (`msg % self.args`) LogRecord의 메시지를 반환하는 함수.


```python
def getMessage(self):
    msg = str(self.msg)
    if self.args:
        msg = msg % self.args
    return msg
```
