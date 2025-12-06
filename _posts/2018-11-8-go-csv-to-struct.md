---
layout: post
title: Go 언어 csv파일 읽어서 구조체로 반환해보기
---

오늘은 Go 언어로 외부의 csv파일을 읽어서 구조체로 반환하는 실습을 진행해보려 합니다.

## 구조체 준비

```go
type testData struct{
	code string
	Title string
	Description string
}
```

우선 데이터를 규칙적으로 넣을 구조체를 만들어줍니다.

```go
var data []testData
```

여러 세트를 for문으로 반복해서 작업하기 위해서 구조체 타입으로 배열을 만듭니다.

## 데이터 준비

```csv
0,제목 1,설명 1
1,제목 2,설명 2
```

구조체 필드에 넣을 데이터를 모아둔 csv 파일을 만들어줍니다.

## 함수 준비

```go
func readCSV(filename string)[]testData{
	return data
}
```

구조체를 반환할 함수를 만들어야 합니다.

## 함수 구현

이제 함수를 구현해보려 합니다.

```go
func readCSV(filename string)[]testData{
	file, _ := os.Open(filename)
	read := csv.NewReader(file)
	read.FieldsPerRecord = -1
```

파일 이름을 인자로 받아서 os 패키지로 해당 csv파일을 열어줍니다.

```go
	var tmp event
	data, _ := read.ReadAll()
	for _, e := range data {
		tmp.code = e[0]
		tmp.Title = e[1]
		tmp.Description = e[2]
		data = append(data,tmp)
    }
```

event 구조체 타입의 tmp 변수를 만들어서 for문으로 해당 임시 구조체 필드에 값을 넣어줍니다.

for문 한번 반복해서 임시 구조체 변수에 한 세트가 완성되면 구조체들의 배열에 append 하여 추가해줍니다.

```go
	file.Close()
	return data
}
```

마지막에는 csv 파일을 열어준 것을 반드시 닫아주고, 만들어둔 구조체 타입의 배열을 반환합니다.

## 핸들러 등록

```go
func httpHandler(handler http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		readCSV("./eventDB.csv")
		handler.ServeHTTP(w, r)
	})
}
```

서버에서 사용하려면 핸들러에 등록해서 위와 같이 사용할 수 있습니다.

위는 eventDB.csv 파일을 읽어오는 예시입니다.

이렇게 코드를 작성하고, 어제 실습한 api서버와 결합하면 csv 파일에서 읽어와서 json 형식으로 반환해주는 것도 작성할 수 있습니다.