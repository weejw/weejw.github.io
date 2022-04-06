---
layout: post
title: TDD(Test Driven Development) (1)
subtitle: Python에서 TDD로 진행해보자. 
author: weejw
categories: python

tags: [tdd, python, development]
---

내용이 많아서 분할해서 진행해야겠다 ;(

[https://testdriven.io/blog/modern-tdd/](https://testdriven.io/blog/modern-tdd/) 를 참고하여 공부했다.

## 어떻게 테스트를 해야하는가?

- 테스트는 기본적으로 아래 3가지에 대한 결과를 제공할 수 있어야한다. <br>
  - GIVEN
  - WHEN
  - THEN

- 단일 테스트로 모든 테스트는 독립적이여야 하며 각 파이프 라인 별로 1회만 진행한다.

## 기본 사용

가장 기본적으로는 아래처럼 테스트가 가능하다. 이를 test 파일로 분할시켜보자.
```python
def sum1(a, b):
    return a + b

def sum0():
    assert sum1(3, 2) == 5
sum0()
```

아래처럼 디렉토리를 나누어 준다. 이후에 위에 존재하는 sum0을 test_sum.py로 이동! 그럼 분리가 된다.<br>
![image](https://user-images.githubusercontent.com/33684393/160510523-1f1b1f54-7d83-42b5-8a46-56b04a3ad30e.png)

```python
from ...sum.sum import sum1


def test_sum():
    assert sum1(2, 3) == 5
```

### pytest 설치 

pytest를 설치하고 tests 아래에 빈 파일 2개를 생성한다 [conftest.py, pytest.ini]
```shell
$ pip install pytest
``` 

그리고 나서 다음 명령어를 입력하면 결과를 출력한다
```shell
python -m pytest tests
```

```shell
S C:\Users\weejw\PycharmProjects\pythonProject\testing_project> python -m pytest tests
================================================ test session starts ================================================
platform win32 -- Python 3.8.10, pytest-7.1.1, pluggy-1.0.0
rootdir: C:\Users\weejw\PycharmProjects\pythonProject\testing_project\tests, configfile: pytest.ini
collected 1 item                                                                                                     

tests\test_sum\test_sum.py .                                                                                   [100%]

================================================= 1 passed in 0.02s =================================================
```


## 실제 어플리케이션에서 사용하기

pydantic을 설치한다. (pydantic은 타입 애너테이션을 사용해 데이터를 검증하고 설정들을 관리하는 라이브러리로 런타임 환경에서 타입을 강제하고 유효하지 않을 시에 에러 발생) <br>
```shell
$ pip install "pydantic[email]"
```

파일 구조는 아래와 같다. <br>

![image](https://user-images.githubusercontent.com/33684393/160511441-4e235593-6a29-461c-b567-38db2f3172af.png)
[https://wookkl.tistory.com/62](https://wookkl.tistory.com/62) <br>

[참고 블로그](https://testdriven.io/blog/modern-tdd/) 에서 코드를 복사하여 models.py에 붙여준다.

아래는 코드의 일부이다. 코드를 보면 알 수 있듯이 pydantic을 이용하여 타입을 체크할 수 있도록 되어있다. 
```python
from pydantic import BaseModel, EmailStr, Field

class Article(BaseModel):
    id: str = Field(default_factory=lambda: str(uuid.uuid4()))
    author: EmailStr
```

tests 폴더 아래에 다음과같이 test_article이라는 폴더와 test_commands를 추가한다. 그리고 [참고 블로그](https://testdriven.io/blog/modern-tdd/) 에서 코드를 복사하여
test_commands.py와 commands.py에 붙여넣는다 :)<br>
![image](https://user-images.githubusercontent.com/33684393/160511827-6cc7b9eb-c0c9-4783-90a5-3a45f36845e1.png)

test_commands.py에는 2가지 test가 존재한다.
test create artilce에서는 assert로 체크하고, already exsists에서는 pytest로 체크하였다. 이후 동일하게 "python -m pytest tests" 로 테스트를 진행할 수 있다.
> def test_create_article()
> 
> def test_create_article_already_exists()

아래는 conftest.py의 일부 코드이다. 보면 autouse=True 라는 부분이 있다. 이를 본문에서는 아래와 같이 설명하고 있다.
> The autouse flag is set to True so that it's automatically used by default before (and after) each test in the test suite. Since we're using a database for all tests it makes sense to use this flag. That way you don't have to explicitly add the fixture name to every test as a parameter.
```python
@pytest.fixture(autouse=True)
def database():
```

## REFERENCES
[https://ichi.pro/ko/pydantic-choboja-gaideu-204731199577737](https://ichi.pro/ko/pydantic-choboja-gaideu-204731199577737) <br>

