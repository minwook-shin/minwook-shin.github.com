---
layout: post
title: Python 기본 시퀀스 비교 difflib 라이브러리 알아보기
---

오늘은 Python에서 HTML 및 context와 unified diff 정보를 생성할 수 있는 difflib 라이브러리를 알아보려 합니다.

## difflib 설치

파이썬에 기본적으로 설치되어 있습니다.

## context_diff

```python
import difflib
import sys
```

difflib와 sys를 가져옵니다.

```python
original = ['python\n', 'java\n', 'go\n']
edited = ['pythonic\n', 'javachip\n', 'go\n']
```

문자열 리스트들을 만듭니다.

```python
sys.stdout.writelines(difflib.context_diff(original, edited, fromfile='original.py', tofile='edit.py'))
```

리스트들을 문맥별 달라진 제네레이터로 차이점으로 보여줍니다.

## ndiff

```python
import difflib
```

difflib를 가져옵니다.

```python
n = difflib.ndiff("""
python
java
go""".splitlines(keepends=True),

"""
pythonic
javachip
go""".splitlines(keepends=True))
print(''.join(n), end="")
```

문자열의 리스트를 비교하여 제네레이터로 만들어 차이점을 보여줍니다.

## unified_diff

```python
import difflib
import sys
```

difflib와 sys를 가져옵니다.

```python
original = ['python\n', 'java\n', 'go\n']
edited = ['pythonic\n', 'javachip\n', 'go\n']
```

문자열 리스트들을 만듭니다.

```python
sys.stdout.writelines(difflib.unified_diff(original, edited, fromfile='before.py', tofile='after.py'))
```

문자열 리스트을 비교하여 제네레이터로 차이점을 보여줍니다.

## compare

```python
import difflib
```

difflib를 가져옵니다.

```python
text_1 = """
#include <iostream>
int main() 
{
    return 0;
}""".splitlines(keepends=True)

text_2 = """
#include <iostream>
using namespace std;
int main() 
{
    cout << "Hello, World!";
    return 0;
}""".splitlines(keepends=True)
```

문자열을 줄 별로 나누어 변수에 대입합니다.

```python
diff = difflib.Differ()

result = list(diff.compare(text_1, text_2))
print(result)
```

Differ 객체로 두 문자열을 비교하는 diff.compare를 사용할 수 있습니다.