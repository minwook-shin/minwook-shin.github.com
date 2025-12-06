---
layout: post
title: Python 영상 편집하고 gif 생성할 수 있는 MoviePy 라이브러리 알아보기
---

오늘은 Python에서 영상을 클립으로 만들어서 편집하거나 섞거나, 움직이는 gif를 만들 수 있는 MoviePy 라이브러리를 알아보려 합니다.

## MoviePy 설치

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
pip install -U Flask
```

해당 라이브러리는 Flask 서버에서 적용할 수 있으므로 Flask 먼저 설치합니다.

```
pip install moviepy
```

pip로 MoviePy를 설치합니다.

## gif

numpy로 그린 sin 곡선을 gif로 만드려고 합니다.

```python
import matplotlib.pyplot as plt
import numpy as np
from moviepy.editor import VideoClip
from moviepy.video.io.bindings import mplfig_to_npimage
```

그래프를 그리는 matplotlib과 numpy, 그리고 moviepy를 가져옵니다.

```python
x = np.linspace(-np.pi, np.pi)
fig, ax = plt.subplots()
```

구간을 분할해주고, figure를 생성해줍니다.

```python
def make_frame(t):
    ax.clear()
    ax.plot(x, np.sin(x + 2 * np.pi / 2 * t))
    return mplfig_to_npimage(fig)
```

matplotlib을 RGB로 변환하는 함수를 작성합니다.

```python
animation = VideoClip(make_frame, duration=2)
animation.write_gif('matplotlib.gif', fps=60)
```

VideoClip으로 움직이는 gif 파일을 만들 수 있습니다.

위 예제대로 작성하면 흔들리는 sin 곡선을 60프레임으로 감상할 수 있습니다.

## text movie

문자열로만으로 영상을 만드려고 합니다.

scipy가 없으면 오류가 발생하므로 scipy를 설치합니다.

```
pip install scipy
```

그리고 imagemagick이 설치되어 있지않아도 오류가 발행하므로 imagemagick도 설치합니다.

```
brew install imagemagick
```

맥의 경우에 brew로 쉽게 설치할 수 있습니다.

```python
from moviepy.editor import *
from moviepy.video.tools.segmenting import findObjects
```

moviepy를 가져옵니다.

```python
text_clip = TextClip('Hello, World!', color='white', kerning=5, fontsize=100)
composite_video_clip = CompositeVideoClip([text_clip.set_pos('center')], size=(1000, 1000))
```

TextClip으로 문자열을 영상으로 만듭니다.

```python
def in_effect(pos):
    d = lambda t: 1.0 / (0.3 + t ** 8)
    return lambda t: pos + 400 * d(t)


def out_effect(pos):
    d = lambda t: max(0, t)
    return lambda t: pos + 400 * d(t)
```

글자가 나타나고 들어가는 효과를 함수를 만들 수 있습니다.

```python
def move(letters, func_pos):
    return [i.set_pos(func_pos(i.screenpos)) for i in letters]

clips = [CompositeVideoClip(move(findObjects(composite_video_clip), func_pos), size=(1000, 1000)).subclip(0, 5)
         for func_pos in [in_effect, out_effect]]
```

문자들을 미리 작성해둔 in_effect와 out_effect 함수들을 적용합니다.

```python
video_clip = concatenate_videoclips(clips)
video_clip.write_videofile('hello_world.avi', fps=60, codec='mpeg4')
```

비디오 클립으로 만들어서 hello_world라는 avi 파일로 저장합니다.

위 예제 코드대로 수행되면 Hello, World!라는 문자열이 60 프레임으로 나타났다가 다시 화면 밖으로 나갑니다.