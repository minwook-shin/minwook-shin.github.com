---
layout: post
title: R 문자열과 숫자 실습해보기
---

오늘은 R로 추출한 데이터를 가지고 문자열과 숫자들을 가지고 실습해보려 합니다.

```r
# 문자열 데이터 처리
sample_text = data.frame(test1 = c("abc_sdfsdf", "abc_xxdfsfsfs", "ccd"),
                         test2 = 1:3, stringsAsFactors = FALSE)
```

우선 실습 데이터를 위해 처음 데이터 묶음은 벡터로하고, 두번째는 범위 지정으로 합니다.

그리고 stringsAsFactors는 FALSE로 둡니다.

```r
sample_text
View(sample_text)
```

그리고나서 위 데이터를 출력해봅니다.

View 명령을 쓰면 GUI로 열립니다.

```r
# 문자 갯수 세기 
nchar(sample_text[1,1])
```

1번째 행, 1번째 열의 문자 갯수가 출력됩니다.

```r
# 특정 문자 위치 알기
which(sample_text[,1] == "ccd")
```

sample_text의 1번째 열의 모든 행에 "ccd"라는 문자열이 있는 위치를 알려줍니다.

```r
# 대소문자 변환
tolower(sample_text[1,1])
toupper(sample_text[1,1])
```

tolower는 모두 소문자로 변환되고 출력되며, toupper는 모두 대문자로 변환되고 출력됩니다.

```r
# 문자열 결합
paste(sample_text[1,1],sample_text[2,1])
paste(sample_text[1,1],sample_text[2,1],sep = ";")
```

두개의 문자열을 결합할 때는 각각 위치를 지정해주고 paste로 붙여주면 됩니다.

구분자를 인자로 추가하면 문자열 사이에 구분자도 같이 붙습니다.

```r
# 특정 위치 문자 추출
substr(sample_text[,1],1,3)
```

특정 위치의 문자열도 편집할 수 있습니다.

```r
#숫자 생성
num = c(1:1000)
num
# 특정 시드
set.seed(50)
# 30개 추출
num = sample(num,30)
num
# 역순 정렬
num = num[order(-num)]
# 정렬
num = num[order(num)]
num
```

그리고 숫자를 범위 지정하여 생성한 다음, 설정된 시드로 30개의 숫자를 난수로 추출할 수도 있습니다.

order 명령으로 숫자들을 정렬할 수 있으며, 음수를 이용하여 역순으로도 바꿀 수 있습니다.

```r
# 최소 값
min(num)

# 최대 값
max(num)

# 평균값
mean(num)

# 중앙 값
median(num)
```

벡터로 묶인 변수에서 최소, 최대, 평균, 중앙 값을 구할 수 있습니다.

```r
# 최빈 값
# 사용자 정의 함수도 가능
mode = function(x){
  ux = unique(x)
  ux[which.max(tabulate(match(x,ux)))]
}
mode(c(1,4,5,2,6,1,1,6,1,1))
```

위는 최빈값을 구하는 사용자 정의 함수이며, 

R에서도 함수를 만들어서 위와 같이 이용할 수 있다는 것을 의미합니다.


```r
unique(c(1,4,5,2,6,1,1,6,1,1))
```

여기서 잠시 unique 명령을 짚고 넘어가자면, unique 함수는 지정된 벡터에서 중복된 값은 삭제해줍니다.

```r
# 분산
var(num)

# 표준편차
sd(num)
```

벡터의 분산과 표준편차를 구할 수 있습니다.

```r
# 결측치가 포함된 값의 평균
# NA는 들어가면 무조건 결과값이 NA로 출력됩니다.
mean(c(1:2,NA,20))
mean(c(1:2,NA,20),na.rm = TRUE)
10+NA
10-NA
10*NA
10/NA
```

결측치가 포함된 값 평균을 출력해주는 방식입니다.

NA가 포함된 계산에는 전부 NA로 출력되므로, na.rm 인자를 TRUE로 둬야 결측치가 포함되도 정확한 계산을 할 수 있습니다.