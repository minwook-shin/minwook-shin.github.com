---
layout: post
title: Python 자동 summarization sumy 라이브러리 알아보기
---

오늘은 Python에서 자동으로 summarization 작업을 해줘서 텍스트와 html 코드를 요약하는 sumy 라이브러리를 알아보려 합니다.

## sumy 설치

우선 virtualenv로 파이썬 환경을 분리해줍니다.

```
pip3 install virtualenv
```

```
virtualenv -mvenv env
```

env라는 이름의 가상 환경을 생성합니다.

```
source env/bin/activate
```

가상환경을 폴더에서 활성화합니다.

```
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install sumy
```

pip로 sumy를 설치합니다.

```
python -c "import nltk; nltk.download('punkt')"

pip install numpy
```

Punkt Tokenizer를 내려받고, numpy도 설치해줍니다.

## hello world 예제

hello world라는 문장을 위키피디아로 검색한 문서를 sumy로 본문을 요약하여 출력해보았습니다.

```python
from sumy.nlp.stemmers import Stemmer
from sumy.nlp.tokenizers import Tokenizer
from sumy.parsers.html import HtmlParser
from sumy.summarizers.lsa import LsaSummarizer
from sumy.utils import get_stop_words
```

sumy에서 stemmers, tokenizers, parsers, summarizers 등을 가져옵니다.

```python
url = "https://en.wikipedia.org/wiki/%22Hello,_World!%22_program"
parser = HtmlParser.from_url(url, Tokenizer("english"))
```

Hello,_World! 프로그램에 대하여 기술한 위키피디아 주소를 가져와서 Tokenizer로 파싱합니다.

```python
stemmer = Stemmer("english")
summarizer = LsaSummarizer(stemmer)
summarizer.stop_words = get_stop_words("english")
```

Stemmer 객체로 Lsa Summarizer 를 사용합니다.

```python
for sentence in summarizer(parser.document, 8):
    print(sentence)
```

8줄로 요약하면 아래와 같이 요약되어 출력됩니다.

> message being displayed through long-exposure light painting with a moving strip of LED lightsA "Hello, World!"
is also traditionally used in a sanity test to make sure that a computer language is correctly installed, and that the operator understands how to use it.
While small test programs have existed since the development of programmable computers , the tradition of using the phrase "Hello, world!"
This claim is supported by the archived notes of the inventors of BCPL, Prof. Brian Kernighan at Princeton and Martin Richards at Cambridge.
Mark Guzdial and Elliot Soloway have suggested that the "hello, world" test message may be outdated now that graphics and sound can be manipulated as easily as text.
Languages otherwise capable of Hello, World (Assembly, C, VHDL ) may also be used in embedded systems , where text output is either difficult (requiring additional components or communication with another computer) or nonexistent.
For devices such as microcontrollers , field-programmable gate arrays , and CPLD 's, "Hello, World" may thus be substituted with a blinking LED , which demonstrates timing and interaction between components.
While of itself useless, it serves as a sanity check and a simple example to newcomers of how to install a package.
