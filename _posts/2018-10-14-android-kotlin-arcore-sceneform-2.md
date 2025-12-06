---
layout: post
title: 코틀린 프로젝트로 안드로이드에서 ARcore sceneform 사용해보기 2
---

오늘도 어제에 이어서 arcore sceneform으로 renderables를 만들어 사용해보려 합니다.

arcore의 런타임 체크와 같은 사전 설정은 이전 포스트에서 보실 수 있습니다.

## ar 레이아웃

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.google.ar.sceneform.ux.ArFragment"
        android:id="@+id/ux_fragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

## Asset 생성 

우선 안드로이드 스튜디오에 Google Sceneform Tools 플러그인을 설치해야 합니다.

그리고 sampledata 폴더에 준비해둔 모델링 파일을 넣어줍니다.

마지막으로 해당 폴더에서 Import Sceneform Asset으로 프로젝트에 import해줍니다.

```java
apply plugin: 'com.google.ar.sceneform.plugin'

sceneform.asset('sampledata/models/andy.obj',
        'default',
        'sampledata/models/andy.sfa',
        'src/main/res/raw/andy')
```

app.gradle 파일에 위와 같이 업데이트되어 있으면 모델링을 생성하는 단계가 끝났습니다.

이제 sfb 파일을 열어보면 모델링을 안드로이드 스튜디오에서 미리 볼 수 있습니다.

## ar fragment 준비

```kotlin
private var arFragment: ArFragment? = null
arFragment = childFragmentManager.findFragmentById(R.id.ux_fragment) as? ArFragment
```

## renderable 준비

```kotlin
private lateinit var andyRenderable: ModelRenderable

ModelRenderable.builder()
    .setSource(this, R.raw.andy)
    .build()
    .thenAccept { renderable -> andyRenderable = renderable }
    .exceptionally {
        App.INSTANCE.toast("Unable to load")
        null
    }
```

ModelRenderable 타입의 renderable 객체를 준비하고, 준비한 Asset을 넣어 renderable로 빌드해줍니다.

빌드에 성공하면 renderable 객체에 값이 담기게 됩니다.

```kotlin
ViewRenderable.builder()
    .setView(this.activity, R.layout.widget)
    .build()
    .thenAccept { renderable -> 
        testRenderable = renderable
        val button : Button = testRenderable.view.findViewById(R.id.button) as Button
        button.setOnClickListener {

        }
    }
```

만약 버튼을 포함하는 레이아웃을 ar에 띄우고 싶다면, Renderable의 뷰에서 id를 가져와서 클릭에 관한 이벤트 리스너를 설정해주면 됩니다.

## 바닥 클릭 이벤트

```kotlin
arFragment?.setOnTapArPlaneListener { hitResult: HitResult, _: Plane, _: MotionEvent ->
    val anchor = hitResult.createAnchor()
    val anchorNode = AnchorNode(anchor)
    anchorNode.setParent(arFragment?.arSceneView?.scene)

    val andy = TransformableNode(arFragment?.transformationSystem)
    andy.setParent(anchorNode)
    andy.renderable = andyRenderable
    andy.select()
}
```

arFragment에서 카메라를 흔들어 클릭할 수 있는 공간이 나타나면, 그 곳에서 클릭할 때의 이벤트 리스너를 설정할 수 있습니다.

그 공간의 바닥에 모델링을 지지할 수 있는 역할을 anchor라고 불리는데 anchor의 자식이 ModelRenderable 타입의 모델링 객체가 되야 클릭할 때에 모델링이 나타나게 됩니다.

이제 앱을 빌드하고 arcore을 즐길 수 있습니다.