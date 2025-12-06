---
layout: post
title: Python svg 파일로 차트를 저장하는 Pygal 라이브러리 알아보기
---

오늘은 Python을 가지고 svg 파일로 차트를 저장할 수 있는 Pygal 패키지에 대하여 알아보려 합니다.

## Pygal 설치

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
pip install pygal
```

pip로 Pygal을 설치합니다.

## Bar

```python
import pygal

bar_chart = pygal.Bar()
bar_chart.title = "bar"
bar_chart.add('test', [0, 1, 2, 3, 4, 5, 6, 7, 8])
bar_chart.render_to_file('bar.svg')
```

bar라는 제목을 가진 바 형식의 차트를 만들 수 있습니다.

추가로 svg 파일로 렌더링할 수도 있습니다.

## Stacked Bar

```python
import pygal

bar_chart = pygal.StackedBar()
bar_chart.title = "Stacked Bar"
bar_chart.add('test', [0, 1, 2, 3, 4, 5, 6, 7, 8])
bar_chart.render_to_file('Stacked_Bar.svg')
```

Stacked Bar라는 제목을 가진 스택 바 형식의 차트를 만들 수 있습니다.

추가로 svg 파일로 렌더링할 수도 있습니다.

## Horizontal Stacked Bar

```python
import pygal

bar_chart = pygal.HorizontalStackedBar()
bar_chart.title = "Horizontal Stacked Bar"
bar_chart.add('test', [0, 1, 2, 3, 4, 5, 6, 7, 8])
bar_chart.render_to_file('Stacked_Bar.svg')
```

Horizontal Stacked Bar라는 제목을 가진 수평 스택 바 형식의 차트를 만들 수 있습니다.

추가로 svg 파일로 렌더링할 수도 있습니다.

## SVG

```python
chart.render()
```

바이트로 이루어진 svg를 반환합니다.

## File

```python
chart.render_to_file('chart.svg')
```

svg 파일로 반환합니다.

## PNG

```python
chart.render_to_png('chart.png')
```

png 파일로 반환합니다.

```
apt-get install libxml2-dev libxslt1-dev python-dev
```

검은 색으로 출력될 때는 lxml을 설치합니다.

우분투에서는 위와 같이 설치할 수 있습니다.

## Browser

```python
chart.render_in_browser()
```

lxml이 설치되어 있는 상태에서 기본 브라우저로 볼 수 있습니다.

## Flask

Flask 애플리케이션을 이용하여 웹으로 차트를 출력할 수 있습니다.

```python
@app.route('/charts/')
def line_route():
   return render_template( 'charts.html', chart = chart)
```

라우터를 생성해줘야 합니다.

```html
<div id="chart">
  <embed type="image/svg+xml" src="{{" chart|safe }} />
</div>
```
