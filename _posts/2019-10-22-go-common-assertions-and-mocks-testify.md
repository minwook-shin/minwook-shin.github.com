---
layout: post
title: Golang assert와 mock Testify 라이브러리 알아보기
---

오늘은 Golang으로 Assert와 Mock 그리고 Testing suite를 만들 수 있는 Testify 라이브러리를 알아보려 합니다.

## Testify 설치

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

go get으로 Testify 패키지를 설치합니다.

```
go get github.com/stretchr/testify
```

홈의 go 폴더에 Testify 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package test
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언하지 않고 테스트를 위해 다른 이름을 적습니다.

```go
import (
	"testing"

	"github.com/stretchr/testify/assert"
	"github.com/stretchr/testify/mock"
	"github.com/stretchr/testify/suite"
)
```

testing과 testify의 assert, mock, suite를 가져옵니다.

```go
func TestEqual(t *testing.T) {
	assert.Equal(t, 10, 10, "equal")
	assert.NotEqual(t, 1, 10, "not equal")
}
```

테스트 함수를 만들어서 값들이 같은지에 대하여 테스트할 수 있습니다.

```go
type MockObj struct {
	mock.Mock
}
```

Mock 구조체를 만듭니다.

```go
func (mock *MockObj) testFunc(number int) (bool, error) {
	args := mock.Called(number)
	return args.Bool(0), args.Error(1)
}
```

Mock 구조체로 메소드를 만듭니다.

```go
func TestFunc(t *testing.T) {
	testObj := new(MockObj)
	testObj.On("testFunc", 1).Return(true, nil)
	testObj.AssertExpectations(t)
}
```

Mock 구조체로 메소드를 가지고 테스트할 수 있는 함수를 만듭니다.

```go
type TestSuite struct {
	suite.Suite
	Variable int
}
```

Testing suite 구조체를 만듭니다.

```go
func (suite *TestSuite) SetupTest() {
	suite.Variable = 1
}
func (suite *TestSuite) TestExample() {
	assert.Equal(suite.T(), 1, suite.Variable)
}
```

Variable을 1로 설정하고 테스트 함수에서 Equal로 값이 같은지 테스트합니다.

```go
func TestExampleTestSuite(t *testing.T) {
	suite.Run(t, new(TestSuite))
}
```

suite.Run로 모든 Testing suite를 수행합니다.