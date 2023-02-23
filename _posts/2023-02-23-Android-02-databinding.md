---
layout: single
title: "[Android] DataBinding"
excerpt: "Android DataBinding 기본 정리"
tag: [Android]
categories: android
commnets: true
author_profile: true
published: true
---


# Android DataBinding
Android DataBinding 라이브러리는 기존 findViewById()를 사용하여 view에 data를 표현하는 방식과 달리 선언적 형식으로 레이아웃의 UI 구성요소를 앱의 데이터 소스와 결합할 수 있도록 한다.


<br/>

- **_기존 방식_**

```java
// TestActivity.java
TextView textView = findViewById(R.id.sample_text);
textView.setText(viewModel.getUserName());
```

- **_DataBinding_**

```xml
<!-- test_activity.xml -->
<TextView
	...
	android:text="@{viewModel.userName}" />
```

<br />

---

## Build.gradle에 추가
DataBinding을 사용하려먼 먼저 build.gradle(Module)에 요소를 추가해야 한다.
```groovy
android {
	...
	dataBinding {
		enabled = true
	}
}
```

<br />

---
## 기본 사용 방법
DataBinding를 이용한 xml 작성 방법은 layout 태그를 루트 태그로 만들고 data 태그와 Layout Container를 작성하면 된다.

(기존 xml의 view 루트 요소를 layout 태그로 감싸야 한다)

```xml
<?xml version="1.0" encoding="utf-8"?>
    <layout xmlns:android="http://schemas.android.com/apk/res/android">
       <data>
           <variable name="user" type="com.example.User"/>
       </data>
       <LinearLayout
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.firstName}"/>
           <TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.lastName}"/>
       </LinearLayout>
    </layout>
```

data 태그 내 레이아웃에서 사용할 속성을 선언

```xml
<variable name="user" type="com.example.User" />
```

레이아웃 내 표현식은 ‘@{}’ 구문을 사용 (데이터의 입력이 필요 할 경우 ‘@={}’를 사용)

<br/>

---

## DataBinding 연산자
databinding 시 사용 가능한 연산자

- 산술 `+ - / * %`
- 문자열 연결 `+`
- 논리 `&& ||`
- 바이너리 `& | ^`
- 단항 `+ - ! ~`
- 전환 `>> >>> <<`
- 비교 `== > < >= <=`(`<`는 `&lt;`으로 이스케이프 처리해야 함)
- `instanceof`
- 그룹화 `()`
- 리터럴 - 문자, 문자열, 숫자, `null`
- 변환
- 메서드 호출
- 필드 액세스
- 배열 액세스 `[]`
- 삼항 연산자 `?:`

※ Null 병합 연산자

null 병합 연산자 ( ?? )는 왼쪽 피연산자가 null이 아니면 왼쪽 피연산자를 선택하고 null이면 오른쪽 피연산자를 선택한다

```xml
android:text="@{user.firstName ?? ``}"
```

<br />

>
><br />
> 💡 android:text=”@={}” 와 같이 양방향 데이터바인딩을 사용 중 Null 병합 연산자를 사용하면 오류가 발생한다
>
> <br />


<br />

---
## 이벤트 처리
이벤트 처리방식으로는 크게 2가지 방식이 있다

<br />

- **_메서드 참조_**
<br />
표현식에서 리스너  메소드의 메소드명과 일치하는 메소드를 참조할 수 있다.
    
    ```xml
    android:onClick="@{handlers::onClickUser}"
    ```
    
- **_리스너 결합_**
<br />
이벤트가 발생할 떄 계산되는 Lambda 표현식이다.
    
    ```xml
    android:onClick="@{(view) -> handlers.onClickUser(view)}"
    ```

<br/>

참고 : [https://developer.android.com/topic/libraries/data-binding](https://developer.android.com/topic/libraries/data-binding)