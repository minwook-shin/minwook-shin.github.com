---
layout: post
title: 코틀린으로 작성한 안드로이드 엑셀 읽고/쓰기 대하여 알아보기
---

오늘은 안드로이드에서 코틀린으로 엑셀을 읽고 쓰는 방법에 대하여 알아보려 합니다.

코틀린으로 시도하는 것이여서 apache poi의 wrapper인 KExcelAPI를 써보겠습니다.

## 설정

```java
repositories {
        maven { url 'https://raw.githubusercontent.com/webarata3/maven/master/repository' }
    }
}

dependencies {
    implementation 'link.webarata3.kexcelapi:kexcelapi:0.5.1'
}
```

repositories와 dependencies를 채워주며 KExcelAPI 사용하기 위한 준비를 합니다.

이 KExcelAPI에는 apache poi가 포함되어 있기 때문에 따로 해줄 것은 없습니다.

## excel 파일 생성

```kotlin
private var workbook = HSSFWorkbook()
private var sheet =workbook.createSheet()

private val file = File(getExternalFilesDir(null), "text.xls")
```

새로운 시트를 만들고, java.io.File을 이용하여 기록할 파일을 만들어 줍니다.

createSheet()를 중복해서 호출하면 호출한 만큼 새로운 시트가 생깁니다.

## 쓰기

```kotlin
import link.webarata3.kexcelapi.*
import org.apache.poi.hssf.usermodel.HSSFWorkbook
```

kexcelapi의 get과 kexcelapi의 set를 쓰기 위해 kexcelapi 패키지를 가져오고, wrapper된 org.apache.poi 패키지도 가져옵니다.

```kotlin
sheet[1,1] = "text"
sheet["A1"] = "text"
val os = FileOutputStream(file)
workbook.write(os)
```

kexcelapi의 set을 이용하여 쉽게 행과 열로 데이터를 쓸 수 있고, 이름으로도 쓸 수 있습니다.

## 읽기

```kotlin
import link.webarata3.kexcelapi.*
import org.apache.poi.hssf.usermodel.HSSFWorkbook
```

kexcelapi의 get과 kexcelapi의 set를 쓰기 위해 kexcelapi 패키지를 가져오고, wrapper된 org.apache.poi 패키지도 가져옵니다.

```java
longToast(sheet[text].toStr())
```

코틀린 anko에서 사용할 수 있는 longToast 메소드를 사용하여 간단히 엑셀 파일에 접근해서 읽을 수 있습니다.