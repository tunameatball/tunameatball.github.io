---
layout: single
title: "[Android] Context?"
excerpt: "Android Context 기본 정리"
tag: [Android]
categories: android
commnets: true
author_profile: true
published: true

sitemap :	
  changefreq : daily	
  priority : 1.0	
---


# Android Context
Android Context는 어플리케이션의 현재 상태를 나타내는 역할을 하며, 앱이 작동하는 맥락을 의미합니다.
>
><br />
> 어플리케이션 환경에 대한 전역 정보 인페이스, Android System에 의해 제공되는 추상클래스이다.
> 어플리케이션별 리소스 및 클래스에 액세스할 수 있을 분만 아니라 Activity, Broadcast와 Intent수신 등과 같은 어플리케이션 수준의 작업 호출이 가능하다.
>
><br />


<br/>

---
### Context의 종류
두 종류의 Context가 존재하기 때문에 용도에 맞는 Context를 사용해야 한다.
- Application Context
- Activity Context

<br />

---
### Application Context
싱글톤 인스턴스
이 Context는 Application Lifecycle을 따른다. 오랫동안 지속되거나 특정 Activity에만 쓰이는 객체가 아닌 앱 전역에서 사용될 객체에 사용하기 적합하다.
Application 내에 싱글톤 객체를 만들 때 사용하면 된다. 이 때 Activity Context를 넘겨주게 되면 Activity에 대한 참조를 유지하게 되어 Garbage Collected 되지 않는다 (Memory leak 발생)

<br />

---
### Activity Context
Activity Lifecycle을 따르며 Activity Scope나 Lifecycle이 같은 객체가 Context가 필요한 경우 사용하기 적합하다


<br />

---
### Application Context를 사용하면 안되는 경우
Application Context는 Activity Context가 지원하는 모든 것을 지원하지는 않는다.
GUI 관련 동작에 있어서 Application Context는 오류가 발생될 확률이 높다.

<br />

---
### 요약
- 어플리케이션의 현재 상태를 갖고 있음
- 시스템이 관리하고 있는 Actvity, Application의 정보를 얻기 위해 사용
- 안드로이드 시스템 서비스에서 제공하는 API(리소스, DB, SharedPreferences 등)에 접근하기 위해 사용
- Activity, Application Class는 Context Class를 상속받는다 
- Activity에서 사용된다면 Activity Context를, Application 전역에서 사용된다면 Application Context를 사용하면 된다.
