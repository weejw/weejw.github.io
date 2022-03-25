~~---
layout: post
title: 파이썬 사용 시 메모리 최적화 
subtitle: 파이썬 어플리케이션 이용 시 메모리를 어떻게 최적화할 수 있을까? 
author: weejw
categories: Data Pipeline

tags: [python, memory]
---

전에 TB급 데이터를 다루면서 메모리 관리가 얼마나 중요한지 알게되었다. 툭하면 파이프라인이 메모리로 터지곤했다...ㅠㅠ<br>

그러다가 좋은 글을 하나 발견했다. 바로 [Optimizing Memory Usage in Python Applications](https://martinheinz.dev/blog/68) 이 글이다. 첫 문장에 바로 나같은 사람을 지칭하고있었다. <br>
> arely is anyone concerned with memory consumption, well, until they run out of RAM.


## 왜 메모리를 절약해야할까?
굉장히 직관적인 이유를 말하고 있었다. 돈. 돈은 매우 중요하다 인력을 쓰든, cloud를 쓰든 뭐든 돈이니...<br>
> One simple reason is money. 

두 번째로는 다음과 같다. data가 mass를 가지고있다는게 무슨 말일까?<br> 
> Another reason is the notion that "data has mass"

바로 다음에 설명을 한다. 대량의 데이터에 대한 설명을 하는 것이었다. 내가 위에서 겪었던 TB급 데이터같은.. 
이러한 경우는 캐싱을 하거나 RAM에 저장할 수 없다. 나 또한 18TB 하드를 6개 돌려가며 쓰고 그랬답.. 또륵..
이러한 경우 전체적으로 성능이 느려진다. 
> if there's a lot of it, then it will move around slowly.

마지막은 다음과 같다. 여기서 'do'는 'adding more memory' 다. 
> you can't do that if you don't have any memory left on the machine.

## 병목을 찾아야한다. 

### memory_profiler
memory_profiler라는 python tool이 있다. 이 툴은 라인별로 메모리 사용량 체크가 가능하다. 간단하게 코드를 하나 작성하고 위에 @profile 을 적어줘야한다.<br>
```python
@profile
def memory_intensive():
    lis = []


    for i in range(100000):
        lis.append(100)
    print(sum(lis)) 
    del lis


    lis2 = [None] * 100000
    
    for i in range(100000):
        lis2[i] = 100

    print(sum(lis2))
    del lis2

    return True

memory_intensive()
```

그러면 다음과 같이 나온다. 보면 append 할 때 메모리 사용량이 많아진다. 대신 크기를 미리 지정하고 나서 진행하는건 상대적으로 적다 :) <br>
이는 growth pattern! 리스트가 어느정도 이상 차면 resize를 하기때문이다. [시간복잡도로 살펴보는 파이썬 내장 자료형의 효율적인 활용](https://www.youtube.com/watch?v=XXGd_t6YF50&list=PLZPhyNeJvHRlECdmkJ7M8konKB0NhBfve&index=4) 에서 자세하게 이 이유를 알 수있다.
```shell
Filename: test.py

Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
     1   40.523 MiB   40.523 MiB           1   @profile
     2                                         def memory_intensive():
     3   40.523 MiB    0.000 MiB           1       lis = []
     4
     5
     6   42.125 MiB    0.000 MiB      100001       for i in range(100000):
     7   42.125 MiB    1.602 MiB      100000           lis.append(100)
     8   42.129 MiB    0.004 MiB           1       print(sum(lis))
     9   40.660 MiB   -1.469 MiB           1       del lis
    10
    11
    12   41.426 MiB    0.766 MiB           1       lis2 = [None] * 100000
    13
    14   41.426 MiB    0.000 MiB      100001       for i in range(100000):
    15   41.426 MiB    0.000 MiB      100000           lis2[i] = 100
    16
    17   41.426 MiB    0.000 MiB           1       print(sum(lis2))
    18   41.426 MiB    0.000 MiB           1       del lis2
    19
    20   41.426 MiB    0.000 MiB           1       return True
```

### sys.getsizeof()

아래 사진에 나와있듯이 우리가 배운것 처럼 숫자는 4바이트씩 문자열은 1바이트씩 늘어난다. 그러나 리스트내에선 이는 지켜지지 않는다. 조큼은 당연한 내용....?<br>
![image](https://user-images.githubusercontent.com/33684393/160036604-7f4ab1fb-2fc5-4951-bf1b-17f337e0e2ff.png)

### pympler

pympler는 python object에 대한 사이즈에 대해 좀 더 많은걸 얻을 수 있다고 한다. 
 와우!! 리스트 내의 메모리를 알고자 할때 완전 유용할 것 같다.
![image](https://user-images.githubusercontent.com/33684393/160036941-35ab186c-bfed-4631-851c-aa4698828abd.png)


## RAM 절약하기!

첫 번째는 이미 내가 위에서 말한거였다.. 리스트 크기 할당 한뒤에 사용하기 ㅎㅎ;; 근데 이는 워낙 유명해서..<br>

### array 또는 numpy 사용하기
두 번째는 맙소사! array라는 모듈을 사용하기이다. 아래 사진을 보자.<br>
메모리 사용량이 거진 4배나 줄었다! 다음부턴 이 array를 쓰도록해야겠어!! 단, 저장에 지원되는 타입(integer, string.. 등)이 정해져있다고하니 이를 주의해야겠다.<br>
추가적으로 수학적 연산이 많을때는 numpy 배열을 쓰는 것이 효율적이라고 한다! <br>
![image](https://user-images.githubusercontent.com/33684393/160037864-b7feb2a2-be68-4216-bb58-bdbaf9afe2f2.png)

### `__slot__` 
기본적으로 파이썬 object는 instance 속성의 저장을 위해 dict을 사용한다고 한다. 이 dict은 메모리 사용량이 적지않다.<br>
윗 글만 봤을 땐 잘 이해가 안가서 [이 글](https://ddanggle.gitbooks.io/interpy-kr/content/ch10-slots-magic.html) 을 참고했다. 이 글에 있는 예문이 더 직관적이다.<br>
이 방법으로 메모리를 4-50%나 줄이기도 한다고 한다;; 덜덜.. 또는 pypy를 사용하면된다고 한다. 이러한 최적화를 자동으로해준다고 한다. 아~ 이래서 백준에서 pypy를 쓰면 메모리 에러를 잡을 수 있던 것이었구나!<br>
```python
class MyClass(object):
    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```
```python
class MyClass(object):
    __slots__ = ['name', 'class']

    def __init__(name, class):
        self.name = name
        self.class = class
        self.set_up()
    # ...
```

메모리 사용률을 다음과같이 체크해볼 수 있다. 와 진짜 엄첨 줄어든다.. slot 있는게!!! 어메이징!!! 재미따 ㅎㅎ <br>
![image](https://user-images.githubusercontent.com/33684393/160039521-2c62a04a-eff8-46ce-bb1a-39f9dccb03da.png)

마지막으론 문자열 trie structure 이용이다. [https://marisa-trie.readthedocs.io/en/latest/tutorial.html](https://marisa-trie.readthedocs.io/en/latest/tutorial.html) 에서 사용방법이 나와있다. 역시 파이썬은 없는게 없다..<br>

## Not Using RAN At All

데이터를 한 번에 다 불러오지않고 점진적으로 로드해 사용할 수 있다.<br>
### mmap 사용하기

사용법은 아래와 같다. 
* file.fileno()를 이용해 메모리에 파일을 맵핑한다. 이제 이 데이터를 접근할 수있다. 삭제도 가능하다 물론! :)
![image](https://user-images.githubusercontent.com/33684393/160040806-a0c41548-e05a-4dae-a690-84eda5f1e71f.png)

삭제는 위 글에서 나와있듯이 아래처럼 진행할 수 있다.
```python
import mmap
import re

with open("some-data.txt", "r+") as file:
    with mmap.mmap(file.fileno(), 0) as m:
        # Words starting with capital letter
        # blar~blar~(전체 코드는 위 글로 고고~:))

        # Delete first 10 characters
        start = 0
        end = 10
        length = end - start
        size = len(m)
        new_size = size - length
        m.move(start, end, size - end)
        m.flush()
      
    file.truncate(new_size)
```

## REFERENCES
[https://data-newbie.tistory.com/701](https://data-newbie.tistory.com/701) <br>
[https://ddanggle.gitbooks.io/interpy-kr/content/ch10-slots-magic.html](https://ddanggle.gitbooks.io/interpy-kr/content/ch10-slots-magic.html)
