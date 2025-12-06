---
layout: post
title: Python GeoJSON 표준 형식을 구현한 python-geojson 라이브러리 알아보기
---

오늘은 Python으로 GeoJSON 표준 형식을 구현한 라이브러리인 python-geojson을 적용해보려 합니다.

## python-geojson 설치

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
pip install geojson
```

pip로 python-geojson을 설치합니다.

## Point

```python
from geojson import Point

point = Point((126.97689056396484, 37.57880031671566))
print(point)
print(point.is_valid)
```

geojson에서 Point를 가져와서 Geometries 기본 형태인 Point로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
  "type": "Point",
  "coordinates": [126.9906234741211, 37.55083796990428]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.

## MultiPoint

```python
from geojson import MultiPoint

multi_p = MultiPoint([(126.97689056396484, 37.57880031671566), (126.97079658508302, 37.55492071859406)])
print(multi_p)
print(multi_p.is_valid)
```

geojson에서 MultiPoint를 가져와서 Geometries 멀티파트 형태인 MultiPoint로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
    "type": "MultiPoint",
    "coordinates": [
        ...
    ]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.

## LineString

```python
from geojson import LineString

list_str = LineString([(126.95972442626953, 37.575671232633184), (126.99354171752928,37.57607938149231)])
print(list_str)
print(list_str.is_valid)
```

geojson에서 LineString을 가져와서 Geometries 기본 형태인 LineString으로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
  "type": "LineString",
  "coordinates": [
    [126.92419052124023, 37.54430510679532],
    [127.02529907226561, 37.54376067569239]
  ]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.

## MultiLineString

```python
from geojson import MultiLineString

multi_str = MultiLineString([((126.95972442626953, 37.575671232633184), (126.99354171752928,37.57607938149231)),
                             [(126.95972442626953, 37.575671232633184), (126.99354171752928,37.57607938149231)]])
print(multi_str)
print(multi_str.is_valid)
```

geojson에서 MultiLineString을 가져와서 Geometries 멀티파트 형태인 MultiLineString으로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
    "type": "MultiLineString",
    "coordinates": [
        ...
    ]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.

## Polygon

```python
from geojson import Polygon

polygon = Polygon([((126.98212623596191, 37.5774398615325), (126.96847915649414, 37.55839087912287),
                    (126.9938850402832, 37.55621354238461), (126.98212623596191, 37.5774398615325))])
print(polygon)
print(polygon.is_valid)
```

geojson에서 Polygon을 가져와서 Geometries 기본 형태인 Polygon으로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
  "type": "Polygon",
  "coordinates": [
    [
      [126.97774887084961, 37.586486421515175],
      [126.93311691284178, 37.54171902364639],
      [127.0210075378418, 37.54308013122266],
      [126.97774887084961, 37.586486421515175]
    ]
  ]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.

## MultiPolygon

```python
from geojson import MultiPolygon

multi_poly = MultiPolygon([([(126.98212623596191, 37.5774398615325), (126.96847915649414, 37.55839087912287),
                    (126.9938850402832, 37.55621354238461), (126.98212623596191, 37.5774398615325)],),
              ([(127.98212623596191, 37.5774398615325), (127.96847915649414, 37.55839087912287),
                    (127.9938850402832, 37.55621354238461), (127.98212623596191, 37.5774398615325)],)])
print(multi_poly)
print(multi_poly.is_valid)
```

geojson에서 MultiPolygon을 가져와서 Geometries 멀티파트 형태인 MultiPolygon으로 저장할 수 있습니다.

is_valid 필드로 형태가 올바른지 판단할 수 있습니다.

```json
{
  "type": "MultiPolygon",
  "coordinates": [
    ...
  ]
}
```

json 형태로 변환되면 위와 같이 나타낼 수 있습니다.
