---
layout: post
title: 코틀린으로 작성한 안드로이드에서 zxing 라이브러리를 사용해서 바코드 만들기
---

오늘은 안드로이드에서 zxing 라이브러리를 사용해서 바코드 만드는 방법을 코틀린으로 작성해보려 합니다.

## 설정

이전에 안드로이드에서 zxing 라이브러리를 사용해서 바코드를 인식하는 방법은 zxing을 랩핑한 barcodescanner를 썻다면,

이번에는 zxing을 랩핑한 zxing-android-embedded를 쓰려 합니다.

```java
implementation 'com.journeyapps:zxing-android-embedded:3.2.0@aar'
implementation 'com.android.support:appcompat-v7:25.3.1'
```

## 준비

```kotlin
val writer = MultiFormatWriter()
var bitmap: Bitmap? = null
val encoder = BarcodeEncoder()
```

객체와 변수를 만들어서 준비합니다.

## 형식 지정

```kotlin
val matrix : BitMatrix = writer.encode("text", format,300,200)
```

첫 인자는 String 타입의 텍스트를 넣고, 두번째는 만들 바코드 포맷을 넣고, 그 다음에 사진의 크기를 넣어줍니다.

## 바코드 형식

zxing 파일을 살펴보면 지원하는 바코드 형식을 알 수 있습니다.

```java
// supported barcode formats

// Product Codes
public static final String UPC_A = "UPC_A";
public static final String UPC_E = "UPC_E";
public static final String EAN_8 = "EAN_8";
public static final String EAN_13 = "EAN_13";
public static final String RSS_14 = "RSS_14";

// Other 1D
public static final String CODE_39 = "CODE_39";
public static final String CODE_93 = "CODE_93";
public static final String CODE_128 = "CODE_128";
public static final String ITF = "ITF";

public static final String RSS_EXPANDED = "RSS_EXPANDED";

// 2D
public static final String QR_CODE = "QR_CODE";
public static final String DATA_MATRIX = "DATA_MATRIX";
public static final String PDF_417 = "PDF_417";
```

위와 같이 1D, 2D 바코드를 지원하므로 encode 메소드의 인자에 바코드 포맷을 넣어주면 이 형식대로 출력됩니다.

## 바코드 생성

```java
bitmap = encoder.createBitmap(matrix);
```

바코드의 형식, 텍스트, 크기를 지정한대로 바코드를 만들어줍니다.

## 종합

```kotlin
val writer = MultiFormatWriter()
var bitmap: Bitmap? = null
val encoder = BarcodeEncoder()
try {
    val matrix : BitMatrix = writer.encode(text, format,328,194)
    bitmap = encoder.createBitmap(matrix);
} catch (e: WriterException) {
    e.printStackTrace();
}
```

Bitmap 타입의 bitmap 변수를 이제 image view에 출력하게 하면 됩니다.