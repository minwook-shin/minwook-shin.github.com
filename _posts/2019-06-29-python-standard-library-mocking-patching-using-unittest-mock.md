---
layout: post
title: Python 기본 테스트 프레임워크 unittest.mock 라이브러리 알아보기
---

오늘은 Python에서 기본적으로 존재하는 테스트 프레임워크인 unittest 라이브러리의 mock 객체를 알아보려 합니다.

unittest.mock은 반환 값을 지정하고 필요한 속성을 정상적인 방법으로 설정할 수도 있습니다.

## unittest mock 설치

파이썬 3.3에 기본적으로 설치되어 있습니다.

이전 버전의 Python에는 unittest.mock의 백 포트(pip install mock)를 pypi에서 받아 사용할 수 있습니다.

## 예제

```python
from unittest.mock import MagicMock

class testClass():
    def method(parameter_list):
        pass

test_class = testClass()
test_class.method = MagicMock(return_value=10)
test = test_class.method(1, 2, 3, key='value')
print(test)
```

특정 클래스의 mock 메소드를 만들 수 있습니다.

위 코드는 특정 클래스에 mock 메소드를 만들어서 반환 값이 10으로 출력됩니다.

```python
from unittest.mock import Mock

mock = Mock(side_effect=KeyError('key Error'))
try:
    mock()
except KeyError as e:
    print(e)
```

특정 mock을 실행했을 때에 사이드 이펙트를 일으킬 수 있습니다.

위 코드는 키 에러를 출력합니다.

```python
from unittest.mock import patch

with patch.object(testClass, 'method', return_value=None) as mock_method:
    thing = testClass()
    thing.method(1, 2, 3)
print(mock_method)
```

patch로 클래스의 메소드를 mock으로 지정할 수 있습니다.

```python
from unittest.mock import patch

test_dict = {'key': 'value'}
original = test_dict.copy()
with patch.dict(test_dict, {'newkey': 'new_value'}, clear=True):
    print(test_dict)
print(test_dict)
```

특정 범위 내에서만 원하는 내용을 넣고, 그 범위를 벗어나면 다시 복구할 수 있습니다.

```python
from unittest.mock import MagicMock

mock = MagicMock()
mock.__str__.return_value = 'str_magic_value'
print(str(mock))

from unittest.mock import Mock

mock = Mock()
mock.__str__ = Mock(return_value='str_mock_value')
print(str(mock))
```

MagicMock과 Mock을 이용할 수 있습니다.

MagicMock 클래스는 모든 Mock 메소드가 미리 생성된 mock입니다 

```python
from unittest.mock import create_autospec

def function(a):
    pass

mock_function = create_autospec(function, return_value='autospec')
print(mock_function(1))
```

실제 객체랑 동일한 스펙을 가질 수 있게 create_autospec을 사용할 수 있습니다.

만약 a라는 인자 하나만 받는 함수를 mock으로 만든다면 그에 맞는 인자를 넣어줘야 오류가 발생하지 않습니다.