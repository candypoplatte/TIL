---
title: Django Signal Receiver 함수를 별도 모듈로 분리하여 관리하기
date: 2018-11-24
categories:
  - development
---

나는 여태까지 Signal Receiver 함수는 `models.py`안에 작성했다. 그런데 Receiver 함수가 많아지면서 `models.py`의 사이즈가 커졌고, 코드 양이 많아 정작 모델을 파악하기 힘들어졌다.

Django 공식 문서를 찾아보니, Receiver 함수를 Django App의 루트 모듈이나 `models` 모듈에 위치시키는 것을 추천하지 않는다고 한다. (import issue 방지)

그래서 별도 모듈로 분리하여 관리하기로 결정했다.
(세부 사항은 [Django 공식 문서](https://docs.djangoproject.com/en/2.1/topics/signals/#receiver-functions)와 책 `Two Scoops of Django`를 참고하였다.)

## 별도 모듈로 관리하기 (signals.py)

1. Django App 하위에 `signals`라는 이름의 module을 추가한다.
2. 해당 Application의 Configuration Class의 `ready()` method에서 import하도록 한다.
   - `flake8`에 걸리지 않도록 `noqa F401` 주석을 추가한다.

### Example

Django App "sample" 하위에 `signals.py`를 추가한 후, `app.py`를 아래와 같이 작성한다.

```python
# app.py
from django.apps import AppConfig


class SampleConfig(AppConfig):
    name = 'sample'

    def ready(self):
        import sample.signals  # noqa F401

```

## Links

- [Django Signals (Receiver Functions)](https://docs.djangoproject.com/en/2.1/topics/signals/#receiver-functions)