---
layout: post
title: 코틀린으로 작성한 안드로이드 차트 만들기에 대하여 알아보기
---

오늘은 안드로이드에서 차트 그래프를 만드는 라이브러리인 MPAndroidChart를 코틀린으로 작성해보았습니다.

## 설정

```java
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
    }
}

dependencies {
    implementation 'com.github.PhilJay:MPAndroidChart:v3.0.3'
}
```

jitpack.io 저장소를 활성화해주고, 해당 주소를 implementation로 dependencies에 추가해줍니다.

## 레이아웃

```xml
<com.github.mikephil.charting.charts.BarChart
    android:id="@+id/pie"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
/>
```

위 xml 코드는 BarChart를 예시로 추가한 모습입니다.


## 엑티비티

```kotlin
class CountActivity : BaseActivity(){
    private val listData = = ArrayList<BarEntry>()
    listData.add(BarEntry("0f", "100f"))
    listData.add(BarEntry("1f", "200f"))
    }
```

그래프에 넣을 수치를 BarEntry 타입의 listData 리스트에 add로 넣어줍니다.

BarEntry 클래스에는 첫번째 인자로 간격, 두번째 인자로 값이 들어갑니다.

```kotlin
    private fun initLineChart() {
        val xAxis = chart.xAxis
        xAxis.setDrawLabels(false)
        xAxis.position = XAxis.XAxisPosition.BOTTOM
        xAxis.granularity = 1f

        val rightYAxis = chart.axisRight
        rightYAxis.setDrawLabels(false)
    }
```

x축과 y축을 설정합니다.


```kotlin
    private fun setChart(listData: ArrayList<BarEntry>) {
        val dataSet = BarDataSet(listData, getString(R.string.app_name))
        dataSet.color = ContextCompat.getColor(this@CountActivity, R.color.colorPrimaryDark)
        dataSet.valueTextColor = ContextCompat.getColor(this@CountActivity, android.R.color.black)

        val lineData = BarData(dataSet)
        chart.setFitBars(true)
        chart.data = lineData
        chart.invalidate()
    }
}
```

만든 listData를 BarDataSet 메소드에 넣어주어 데이터를 구성합니다.

그리고 해당 차트에 색을 넣을 수도 있습니다.

마지막으로 dataSet을 chart.data에 넣어주고, invalidate() 메소드를 호출하여 새로 고쳐줍니다.

제 개인적인 생각이지만, invalidate() 메소드는 새로 고침이라는 느낌보다는 기존의 세션을 무력화하고 다시 만들어 준다는 느낌이 강한 메소드인 것 같습니다.

이렇게 코딩하고 안드로이드 스튜디오로 빌드하면 정상적으로 들어간 데이터에 대해서는 차트 그래프가 출력되게 됩니다.