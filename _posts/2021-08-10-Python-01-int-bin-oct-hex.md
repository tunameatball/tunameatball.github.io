---
layout: single
title: "[Python] 2진수, 8진수, 16진수 다루기"
excerpt: "파이썬에서 2진수, 8진수, 16진수 다루는 법"
tag: [python]
categories: python
commnets: true
author_profile: true
published: true
typora-copy-images-to: ..\images\2021-08-10
---
  

## 진수별 기본 표현 
```
2진수 : 0b
8진수 : 0o (영어 소문자 O)
16진수 : 0x
```

<br/>

---
## 10진수 -> 2진수, 8진수, 16진수 형 문자열로 변환

<br/>

10진수를 각각 bin(), oct(), hex() 함수를 사용하여 문자열로 표현 가능하다.


```python
number = 82

binary = bin(number)
octal = oct(number)
hexa = hex(number)

print("%s, %s, %s" % (binary, octal, hexa))
```  

실행 결과
```
0b1010010, 0o122, 0x52
```

<br/>

---

## 2진수, 8진수, 16진수 -> 10진수 변환

<br/>

int() 함수를 이용하여 각 진수를 10진수로 변환 가능하다.

```python
binary = int('0b0111011', 2)
octal = int('0o21562', 8)
hexa = int('0xFA21BC', 16)

print("%d, %d, %d" % (binary, octal, hexa))
```
실행 결과
```
59, 9074, 16392636
```

<br/>

---
## format을 이용한 진수 변환

<br/>

format() 함수를 이용하여 진수를 변환할 수 있다.

```python
number = 82

binary = format(number, '#b')
octal = format(number, '#o')
hexa = format(number, '#x')

print("%s, %s, %s" % (binary, octal, hexa))
```
실행 결과
```
0b1010010, 0o122, 0x52
```