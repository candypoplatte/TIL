---
title: 파이썬에서 Thread 간 Shared Data Lock (& RLock)
date: 2018-11-29
categories:
  - development
---

한 프로세스 내 쓰레드들은 (가상) 메모리 내 힙, 스택, 코드 영역을 공유한다.

파이썬의 GIL과는 별개로, 쓰레드간 공유되는 데이터의 경쟁은 데이터를 꼬이게 만들 수 있다.

- ex. 쓰레드 간 context change가 발생할 때, 특정 쓰레드에서 실행되는 함수가 다른 쓰레드에서도 쓰이는 공유 데이터를 수정해버린다면 데이터가 꼬일 수 있다.

## Lock()

python threading 패키지에서는 Lock을 지원한다.

lock을 acquire하면 해당 쓰레드만 공유 데이터에 접근할 수 있고, lock을 release 해야 다른 쓰레드에서 공유 데이터에 접근할 수 있다.

<img src="/TIL/images/lock_explanation.jpeg" width="50%">

## RLock()

"Reentrant Lock"

간혹 lock을 거는 함수가 재귀호출을 하는 경우 쓰레드가 Block되어 Lock을 해제할 수 없게 되어버림

RLock()은 쓰레드가 lock을 취득한 상태에서 lock을 다시 취득하면 lock count를 1 올리면서 즉시 return한다.

"lock 재 획득 문제를 해결"

## 실습

### 1. No Lock

Lock 없이 여러 개의 쓰레드를 띄워서 Shared Data에 접근하도록 함

```python
import threading

_lock = threading.Lock()
class TestClass:
    def __init__(self):
        self.count = 0

def lets_go(i, c):
    print(f'[Thread {i}] Started (id : {threading.get_ident()})')
    for j in range(5):
        c.count = c.count + 1
        print(f'[Thread {i}] {c.count}')

if __name__ == "__main__":
    tc = TestClass()
    for i in range(5):
        th = threading.Thread(target=lets_go, args=[i, tc])
        th.start()
```

결과. 여러 쓰레드가 서로 경쟁하면서 Shared Data에 접근하는 모습을 볼 수 있다. 순서 X

```bash
[Thread 0] Started (id : 123145539284992)
[Thread 0] 1
[Thread 0] 2
[Thread 0] 3
[Thread 1] Started (id : 123145544540160)
[Thread 1] 5
[Thread 0] 4
[Thread 2] Started (id : 123145549795328)
[Thread 2] 8
[Thread 3] Started (id : 123145555050496)
[Thread 1] 6
[Thread 0] 7
[Thread 4] Started (id : 123145560305664)
[Thread 1] 11
[Thread 2] 9
[Thread 3] 10
[Thread 4] 12
[Thread 1] 13
[Thread 2] 14
[Thread 3] 15
[Thread 4] 16
[Thread 1] 17
[Thread 2] 18
[Thread 4] 20
[Thread 3] 19
[Thread 3] 23
[Thread 4] 22
[Thread 2] 21
[Thread 3] 24
[Thread 4] 25
```

### 2. Lock

Lock을 사용. 특정 쓰레드가 작업을 마치기 전 까지 다른 쓰레드가 Shared Data에 접근할 수 없도록 함.

```python
import threading

_lock = threading.Lock()
class TestClass:
    def __init__(self):
        self.count = 0

def lets_go(i, c):
    print(f'[Thread {i}] Started (id : {threading.get_ident()})')
    _lock.acquire()
    for j in range(5):
        c.count = c.count + 1
        print(f'[Thread {i}] {c.count}')
    _lock.release()


if __name__ == "__main__":
    tc = TestClass()
    for i in range(5):
        th = threading.Thread(target=lets_go, args=[i, tc])
        th.start()
```

결과. 쓰레드가 시작되는 것은 자유롭지만 Shared Data를 사용할 때는 Lock이 해제된 후에 사용할 수 있기 때문에 순차적으로 진행되는 모습을 볼 수 있다.

```bash
[Thread 0] Started (id : 123145574633472)
[Thread 0] 1
[Thread 0] 2
[Thread 0] 3
[Thread 0] 4
[Thread 0] 5
[Thread 1] Started (id : 123145574633472)
[Thread 1] 6
[Thread 2] Started (id : 123145579888640)
[Thread 1] 7
[Thread 1] 8
[Thread 1] 9
[Thread 1] 10
[Thread 3] Started (id : 123145585143808)
[Thread 2] 11
[Thread 4] Started (id : 123145574633472)
[Thread 2] 12
[Thread 2] 13
[Thread 2] 14
[Thread 2] 15
[Thread 3] 16
[Thread 3] 17
[Thread 3] 18
[Thread 3] 19
[Thread 3] 20
[Thread 4] 21
[Thread 4] 22
[Thread 4] 23
[Thread 4] 24
[Thread 4] 25
```

### 3. Lock 재획득 (with Lock)

Lock을 두 번 획득하게 함. 

```python
import threading

_lock = threading.Lock()
class TestClass:
    def __init__(self):
        self.count = 0

def lets_go(i, c):
    print(f'[Thread {i}] Started (id : {threading.get_ident()})')
    # Double Lock
    _lock.acquire()
    _lock.acquire()
    for j in range(5):
        c.count = c.count + 1
        print(f'[Thread {i}] {c.count}')
    # Single Release
    _lock.release()


if __name__ == "__main__":
    tc = TestClass()
    for i in range(5):
        th = threading.Thread(target=lets_go, args=[i, tc])
        th.start()
```

결과. 최초 쓰레드에서 Lock을 재획득하는 순간 쓰레드가 Block되어 나머지 쓰레드도 Shared Data에 접근이 안 됨.

```bash
[Thread 0] Started (id : 123145407381504)
[Thread 1] Started (id : 123145412636672)
[Thread 2] Started (id : 123145417891840)
[Thread 3] Started (id : 123145423147008)
[Thread 4] Started (id : 123145428402176)
```

### 4. Lock 재획득 (with RLock)

RLock을 사용하면 Lock을 재획득하더라도 일단 Return되기 때문에 한 쓰레드의 구문이 진행은 되도록 함.

```python
import threading

_lock = threading.RLock()
class TestClass:
    def __init__(self):
        self.count = 0

def lets_go(i, c):
    print(f'[Thread {i}] Started (id : {threading.get_ident()})')
    # Double Lock
    _lock.acquire()
    _lock.acquire()
    for j in range(5):
        c.count = c.count + 1
        print(f'[Thread {i}] {c.count}')
    # Single Release
    _lock.release()


if __name__ == "__main__":
    tc = TestClass()
    for i in range(5):
        th = threading.Thread(target=lets_go, args=[i, tc])
        th.start()
```

쓰레드 0이 Lock을 두 번 걸었기 때문에 나머지 쓰레드들은 Shared Data에 접근할 수 없다. 하지만 쓰레드 0이 완료된 후 띄워진 쓰레드 2, 그리고 쓰레드 2가 완료된 후 띄워진 쓰레드 4는 같은 쓰레드이기 때문에 (id 참고) Lock을 풀지 않아도 구문이 실행된다.

```bash
[Thread 0] Started (id : 123145457262592)
[Thread 0] 1
[Thread 0] 2
[Thread 0] 3
[Thread 0] 4
[Thread 0] 5
[Thread 1] Started (id : 123145462517760)
[Thread 2] Started (id : 123145457262592)
[Thread 2] 6
[Thread 2] 7
[Thread 2] 8
[Thread 2] 9
[Thread 2] 10
[Thread 3] Started (id : 123145467772928)
[Thread 4] Started (id : 123145457262592)
[Thread 4] 11
[Thread 4] 12
[Thread 4] 13
[Thread 4] 14
[Thread 4] 15
```

## Links

- https://timber.io/blog/multiprocessing-vs-multithreading-in-python-what-you-need-to-know/
- https://soooprmx.com/archives/8834