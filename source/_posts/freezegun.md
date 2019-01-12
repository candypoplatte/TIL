---
title: 시간을 테스트할 때는 FreezeGun
date: 2019-01-11
categories:
  - development
---

`timezone.now()`와 같이 현재 시간을 사용하는 파이썬 코드를 시간을 바꿔가면서 테스트하고 싶을 때가 있다.

이 때 사용하면 좋은 라이브러리가 바로 FreezeGun.

데코레이터 등의 형태로 사용할 수 있다.

## 예시

```python
@freeze_time("1955-11-12")
class MyTests(unittest.TestCase):
    def test_the_class(self):
        assert datetime.datetime.now() == datetime.datetime(1955, 11, 12)
```

## 주의 : Default arguments

FreezeGun은 Default Argument를 수정하지는 않는다.

## Links

- https://github.com/spulec/freezegun
