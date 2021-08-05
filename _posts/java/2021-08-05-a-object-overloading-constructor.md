---
layout: post
title: "01.객체/오버로딩/생성자"
author: "SangKyenog Lee"
tags: Java
---

< Java의 정석 >을 공부하고 정리 했습니다. 

## 객체지향이론의 기본 개념
>실제 세계는 사물로 이루어져 있으며, 발생하는 모든 사건들은 사물간의 상호작용

## 객체지향언어
1. 코드의 재사용성
2. 코드의 관리 용이
3. 신뢰성 높은 프로그래밍

### 클래스와 객체

- 클래스의 정의: 객체를 정의 해놓은 것 // 객체의 설계도 또는 틀
    - EX: 붕어빵 기계
- 객체의 정의: 실제로 존재하는 것 // 사물 또는 개념
    - EX: 붕어빵

### 객체와 인스턴스
- 인스턴스화: 클래스로부터 객체를 만드는 과정
- 인스턴스: 어떤 클래스로부터 만들어진 객체를 해당 클래스의 `인스턴스`라고 한다.
```java
class Tv {
    //멤버변수
    String color;
    boolean power;
    int channel;

    //메서드
    void power() {power = !power;}
    void channelUp() { ++channel;}
    void channelDown() { --channel;}
}

class TvTest{
    public static void main(String[] args){
        Tv t; // tv인스턴스 참조 변수 t
        t = new Tv(); //tv인스턴스를 생성
        t.channel = 7;
        t.channelDonw();
    }
}
```

## 변수와 메서드
1. 클래스 변수 // 클래스가 메모리에 올라갈 떄 생성
    - static이 붙음
2. 인스턴스 변수 // 인스턴스가 생성되었을 때 생성
    - static이 붙지 않음
3. 지역 변수 // 변수 선언문이 수행되었을때 생성

```java
class card {
    //인스턴스 변수
    String kind; // 무늬
    int number; // 숫자

    //클래스 변수
    static int width = 100; // 폭
    static int height = 250; // 높이
}
```
1. 클래스 메서드: 객체 생성을 하지 않아도 `.`을 이용해 호출 가능
2. 인스턴스 메서드: 객체 생성을 해야만 호출이 가능하다.

```java
class math {
    long a, b;

    //인스턴수 변수 a, b를 사용하므로 매개변수 필요 x
    long add() { return a + b;}

    //인스턴스 변수 a, b와 상관없이 매개변수만으로 작업
    static long add(long a, long b){ return a + b;}
}
```

## 오버로딩
> 같은 이름의 메서드를 여러 개 정의 하는 것을 말하며, 매개변수의 타입이나 개수가 달라야 한다.

### 조건
1. 메서드 이름이 같아야 한다.
2. 매개변수의 개수 또는 타입이 달라야 한다.

### 장점
1. println을 예로 들면 여러타입의 변수마다 일일이 이름을 정의할 필요가 없다

### 가변인자와 오버로딩
- 가변인자는 매개변수 맨뒤에 놓인다
- `타입... 변수명`으로 선언
```java
String concatenate(String s1, String s2) {}
String concatenate(String s1, String s2, String s3){}
String concatenate(String s1, String s2, String s3, String s4){}
를
String concatenate(String... str){} 로 나타낼 수 있다.
```

## 생성자
> 인스턴스가 생성될 때 호출되는 `인스턴스 초기화 메서드`

### 생성자 조건
1. 생성자의 이름은 클래스의 이름과 같아야 한다.
2. 생성자는 리턴 값이 없다.

```java
Card c = new Card();
```
1. 연산자 new에 의해서 메모리(heap)에 Card클래스의 인스턴스가 생성됨
2. 생성자 Card()가 호출되어 수행됨.
3. 연산자 new의 결과로, 생성된 Card인스턴스의 주소가 반환되어 참조변수 c에 저장됨

### 기본 생성자(default constructor)
- 기본적으로 모든 클래스는 하나 이상의 생성자를 정의 해야함.
- 생성자를 정의하지 않아도 컴파일러가 `기본 생성자`제공

```java
class Data1 {
    int value;
}
class Data2 {
    int value;
    
    Data2(int x){
        value = x;
    }
}
```
해당 예제에서 `Data1 d1 = new Data1();`은 생성 가능하지만 `Data2 d2 = new Data2()`는 생성이 안됨
- data1은 생성자 선언이 안 되어있어 `기본 생성자`로 생성 가능
- data2는 생성자 선언이 되어있으므로 해당 생성자에 맞춰 매개변수를 넣고 생성 해야함

> 기본 생성자가 컴파일러에 의해 추가되는 경우는 클래스에 정의된 생성자가 없을 경우 뿐

### 생성자에서 다른 생성자 호출 - this(), this

1. 생성자의 이름으로 클래스 이름 대신 this를 사용
2. 한 생성자에서 다른 생성자를 호출할 때는 반드시 첫줄에서만 호출이 가능하다.

#### this
> 1. 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어 있다.
> 2. 모든 인스턴스메서드에 지역변수로 숨겨진 채로 존재한다.

```java
Car(String color, String gearType, int door){
        this.color = color; 
        this.gearType = gearType;
        this.door = door;
    }
```

#### this()
> 같은 클래스의 다른 생성자를 호출할 때 사용한다.

```java
Car(){
        this("white", "auto", 4); // Car(String color, string gearType, int door)를 호출
    }
Car(String color, String gearType, int door){
        this.color = color; 
        this.gearType = gearType;
        this.door = door;
    }
```


## 변수의 초기화
> 멤버변수(클래스 변수와 인스턴스 변수)와 배열의 초기화는 선택적이지만, 지역변수의 초기화는 필수적이다.

- 초기화 방법
1. 명시적 초기화
2. 생성자
3. 초기화 블럭

### 명시적 초기화
> 변수를 선언과 동시에 초기화 하는 것

### 초기화 블럭
```java
class InitBlock {
    static { /* 클래스 초기화 블럭 */ }
    { /* 인스턴스 초기화 블럭 */ }

}
```
- 클래스 초기화 블럭은 클래스가 메모리에 처음 로딩될 때 한번 수행
- 인스턴스 초기화 블럭은 생성자와 같이 인스턴스를 생성할 때 마다 수행
