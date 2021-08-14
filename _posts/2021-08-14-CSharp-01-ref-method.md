---
layout: single
title: "[C#] 참조 매개변수와 참조 리턴"
excerpt: "ref 키워드를 이용해 참조 매개변수 전달과 참조 리턴"
tag: [C#]
categories: csharp
commnets: true
author_profile: true
published: true
typora-copy-images-to: ..\images\2021-08-14
---

---

## 일반 매개변수 전달과 참조 매개변수 전달의 차이

일반 매개변수 전달은 데이터를 복사해서 전달하지만 참조 매개변수는 원본 매개변수의 참조를 전달한다.

일반 매개변수 전달은 메소드 안에서 데이터를 조작해도 원본 매개변수는 바뀌지 않지만

참조 매개변수는 메소드 안에서 데이터를 조작하면 원본 매개변수의 데이터도 바뀐다.



<br/>



---

## ref 키워드를 이용한 참조 매개변수 전달

<br/>

C#에서는 ref 키워드를 이용해 참조 매개변수를 전달할 수 있다.

아래는 a와 b의 데이터를 바꾸는 Swap메소드 예시이다.

```c#
static void Swap(ref int a, ref int b)
{
	int temp = b;
    b = a;
    a = temp;
}

/// main
int x = 3;
int y = 4;
Console.WriteLine($"x:{x}, y:{y}");

Swap(ref x, ref y);
Console.WriteLine($"x:{x}, y:{y}");
```

Swap 메소드의 매개변수를 참조형으로 선언해놨기 때문에 메소드를 호출할 때 매개변수 앞에 ref 키워드를 붙여 호출해야 한다.

참조 매개변수를 전달했기 때문에 실제 x와 y값도 변한걸 알 수 있다.

실행결과

```
x:3, y:4
x:4, y:3
```



<br/>

---

## 메소드의 참조 리턴

<br/>



메소드의 결과 또한 참조 타입으로 반환할 수 있다.

방법은 ref로 메소드를 선언하고 return 문이 반환하는 변수 앞에도 ref 키워드를 작성하면 된다.

```c#
class SomeClass
{
    int number = 10;
    
    public ref int getNumber()
    {
        return ref number;
    }       
}
```



<br/>

### 참조를 리턴하는 메소드 호출 방법과 참조 지역변수

<br/>

참조로 리턴하는 메소드는 아래와 같이 호출 할 수 있다.



```c#
// getNumber메소드는 참조를 리턴하도록 구현됐지만, 호출자가 특별한 키워드를 사용하지 않으면 값을 리턴하는 메소드처럼 동작한다.
SomeClass c1 = new SomeClass();
int result1 = c1.getNumber();

// ref 키워드를 이용한 참조 리턴
SomeClass c2 = new SomeClass();
ref int result2 = ref c2.getNumber(); // result2는 참조 지역 변수
```



<br/>

참조 지역변수(참조로컬, ref local)는 return ref를 이용하여 반환된 값을 참조하는데 사용이 된다. 참조 지역변수의 데이터를 수정하면 참조 지역변수에 메소드가 리턴한 변수의 데이터가 수정된다.  





