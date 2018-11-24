---
title: 파이썬은 Call by Object-Reference
date: 2018-11-22
categories:
  - development
---

파이썬에서,

- Mutable 객체는 Call by Reference 처럼 (List, Dictionary, Set 등) 동작한다.
- Immutable 객체는 Call by Value 처럼 (Tuble, String 등) 동작한다.

## Test

테스트를 해 보았다.

### Test #1. Immutable Object

```python
import sys

def f(x):
    print('3', x, id(x), sys.getrefcount(x))
    x = x + 1  # 이 때 Object-Reference 변경 (Local Scope)
    print('4', x, id(x), sys.getrefcount(x))

a = 1
print('1', a, id(a), sys.getrefcount(a))
a = 2  # 이 때 Object-Reference 변경
print('2', a, id(a), sys.getrefcount(a))
f(a)
print('5', a, id(a), sys.getrefcount(a))
```

결과

```bash
1 1 4425389152 764
2 2 4425389184 129  # change!
3 2 4425389184 131
4 3 4425389216 52  # change! (in local scope)
5 2 4425389184 129
```

## Test #2-1. Mutable Object (내부 method 활용)

```python
import sys

def f(x):
    print('3', x, id(x), sys.getrefcount(x))
    x.append(4)
    print('4', x, id(x), sys.getrefcount(x))

a = []
print('1', a, id(a), sys.getrefcount(a))
a.append(3)
print('2', a, id(a), sys.getrefcount(a))
f(a)
print('5', a, id(a), sys.getrefcount(a))
```

결과

```bash
1 [] 4357985800 3
2 [3] 4357985800 3
3 [3] 4357985800 5
4 [3, 4] 4357985800 5
5 [3, 4] 4357985800 3
```

## Test #2-2. Mutable Object (대치)

```python
import sys

def f(x):
    print('3', x, id(x), sys.getrefcount(x))
    x = [4] + x
    print('4', x, id(x), sys.getrefcount(x))

a = []
print('1', a, id(a), sys.getrefcount(a))
a = [3] + a
print('2', a, id(a), sys.getrefcount(a))
f(a)
print('5', a, id(a), sys.getrefcount(a))
```

결과

```bash
1 [] 4385711624 3
2 [3] 4385711944 3
3 [3] 4385711944 5
4 [4, 3] 4385711688 3
5 [3] 4385711944 3
```

## 다시 정리

> 처음에 작성했던 문장을 다시 이해해보자
> 파이썬은 다 Object!

파이썬은 Call by Object-Reference. 

함수의 Parameter로 전달된 직후를 확인하면 Reference가 전달된 것을 볼 수 있다. (Test #1, #2-1, #2-2 참고)

1 . "Immutable Object는 Call by Value **처럼** 동작한다"의 의미

- 함수 내부에 전달된 Parameter에 다른 객체를 대입하면 다른 Object 를 Reference로 가리키게 된다. 
- 이 일이 함수 내부와 같은 Local Scope내에서 일어나게 되면 Reference가 사라지기 때문에 다시 상위 Scope로 복귀하면 Reference의 값은 값은 변하지 않은 상태이다. (Test #1 참고)
- 함수 외부의 변수에 다른 값을 대입했을 때 Object Reference ID가 변경되는 것도 확인

2 . "Immutable Object는 Call by Reference **처럼** 동작한다"의 의미

- 전달된 Parameter역시 Class Instance이기 때문에 내부 method(append)를 활용하면 Object Reference의 교체 없이 Reference Object를 건드려 값을 변화시킬 수 있다. 
- 내부 method를 실행하여 Object를 변경시킬 수 있는 Object Type이 Mutable이기 때문에 Mutable Object는 "Call by Reference 처럼" 작동한다고 말하는 것이다.(Test #2-1 참고)  
- 대신 Object를 대치하게 되면 이 또한 Object-Reference를 변경하는 행위를 하게되고, Local Scope에서 이루어졌다면 상위 Scope에서는 확인 불가. (Test #2-2 참고)

*Clear!*

## Tip

1. 굳이 Immutable Object를 전달하여 함수 내부에서 상태를 변화시킨 후 결과를 얻고 싶다면, Class Instance variable을 활용하면 되겠다!

2. 이왕이면 함수는 Stateless로...

## Links
- [참고한 Blog 글](https://devdoggo.netlify.com/post/python/python_objective_reference/)
