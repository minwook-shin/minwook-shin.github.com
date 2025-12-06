---
layout: post
title: Golang 3D 선 아트 랜더링 ln 라이브러리 알아보기
---

오늘은 Golang으로 3D 선 랜더링을 할 수 있는 ln 라이브러리를 알아보려 합니다.

## ln 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 ln 패키지를 설치합니다.

```
go get github.com/fogleman/ln/ln
```

홈의 go 폴더에 ln 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import "github.com/fogleman/ln/ln"
```

ln을 가져옵니다.

```go
func main() {
	scene := ln.Scene{}
	scene.Add(ln.NewCube(ln.Vector{-1, -1, -1}, ln.Vector{1, 1, 1}))
```

Scene과 single cube를 만듭니다.

```go
	width := 1024.0
	height := 1024.0
	fovy := 50.0
	znear := 0.1
	zfar := 10.0
	step := 0.01
```

width, height, fovy, near, far, step에 대한 랜더링 파라미터를 준비합니다.

```go
	paths := scene.Render(ln.Vector{3, 3, 3}, ln.Vector{0, 0, 0}, ln.Vector{0, 0, 1},
		width, height, fovy, znear, zfar, step)
```

카메라 파라미터와 미리 준비한 랜더링 파라미터로 3D scene을 만듭니다.

```go
	paths.WriteToPNG("WriteToPNG.png", width, height)
```

만든 3D scene을 가지고 넓이와 높이를 지정해서 이미지로 저장합니다.

```go
	paths.WriteToSVG("WriteToSVG.svg", width, height)
```

만든 3D scene을 가지고 넓이와 높이를 지정해서 SVG로 저장합니다.

```go
	paths.WriteToTXT("WriteToTXT.txt")
}
```

만든 3D scene을 가지고 텍스트 파일로 저장합니다.