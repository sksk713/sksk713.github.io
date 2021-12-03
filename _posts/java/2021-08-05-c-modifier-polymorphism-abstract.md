---
layout: post
title: "[자바의 정석] 제어자/다형성/추상클래스"
author: "SangKyenog Lee"
tags: Java
---

< Java의 정석 >을 공부하고 정리 했습니다. 

## 제어자

- 접근제어자
> public, protected, default, private

- 그 외
> static, final, abstract, native ..

### static
- 멤버변수, 메서드, 초기화 블럭에서 사용됨
    - 멤버변수
        - 모든 인스턴스에 공통으로 사용되는 클래스변수
        - 클래스변수는 인스턴스를 생성하지 않고 사용 가능
        - 클래스가 메모리에 로드될 때 생성
    - 메서드
        - 인스턴스를 생성하지 않고도 호출이 가능
        - static메서드 내에서는 인스턴스멤버들을 직접 사용할 수 없다.

> static 메서드로 하는 것이 인스턴스를 생성하지 않고 호출 하므로 더 편리하고 속도가 빠름

### final
- 클래스, 메서드, 멤버변수, 지역변수에 사용됨
    - final로 선언된 클래스는 다른 클래스의 조상이 될 수 없다.
    - final로 선언된 메서드는 오버라이딩을 통한 재정의가 불가능
    - 멤버변수, 지역변수는 변경 불가능한 상수가 됨

### abstract
- 클래스, 메서드에서 사용됨
    - 클래스 내에 추상 메서드가 선언되어 있음을 의미
    - 선언부만 작성하고 구현부는 작성하지 않는 추상 메서드

> abstract 클래스 자체로는 쓸모 없지만, 상속받아서 원하는 메서드를 오버라이딩 하는 방식으로 사용

### 접근제어자
1. private: 같은 클래스 내에서만 접근 가능
2. default: 같은 패키지 내에서만 접근 가능
3. protected: 같은 패키지 내에서, 그리고 다른 패키지의 자손클래스에서 접근이 가능
4. public: 접근 제한 x

- 사용하는 이유
> - 캡슐화
> 1. 외부로부터 데이터를 보호하기 위해
> 2. 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해


## 다형성(polymorphism)
> - 여러 가지 형태를 가질 수 있는 능력
> - 조상클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있다.

```java
class Tv {
    boolean power;
    int channel;

    void power()
    void channelUp()
    void channelDown()
}
class CaptionTv extends Tv{
    String text;
    void caption()
}

Tv t = new CaptionTv(); 
// 상속 관계일때, 조상 클래스 타입의 참조변수로 자손 클래스의 인스턴스를 참조
```
```java
CaptionTv c = new CaptionTv();
Tv t = new Caption();
```
- 두 개의 차이점은 `같은 타입의 인스턴스지`만 `참조변수의 타입`에 따라 사용할 수 있는 멤버의 개수가 달라진다는 점이다.

- 조건: 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.
> - 조상타입의 참조변수로 자손타입의 인스턴스를 참조할 수 있다.
> - 반대로 자손타입의 참조변수로 조상타입의 인스턴스를 참조할 수는 없다.

### 참조변수간의 형변환
> - 자손타입 -> 조상타입 : 형변환 생략
> - 조상타입 -> 자손타입 : 형변환 생략 불가능

```java
Car car = null;
FireEngine fe = new FireEngine();
FireEngine fe2 = null;

car = fe;
fe2 = (FireEngine)car;
```

### instanceof 연산자
> - 참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위함
> - 연산의 결과로 boolean값을 리턴함

```java
void doWork(Car c){
    if(c instanceof FireEngine){
        ~~~~
    }
}
```
- 상속받은 조상클래스가 있다면 해당 조상클래스와 instanceof 연산을 해도 true가 나옴
> 어떤 타입에 대한 instanceof연산의 결과가 true라는 것은 검사한 타입으로 형변환이 가능하다는 것을 뜻함.

## 여러 종류의 객체를 배열로 다루기
```java
Product p1 = new Tv();
Product p2 = new Computer();
Product p3 = new Audio();

Product[] p = new Product[3];
p[0] = new Tv();
p[1] = new Computer();
p[2] = new Audio();

```

## 추상클래스
> 클래스가 설계도라면 추상클래스는 미완성 설계도라고 할 수 있다.
- 상속을 통해 자손클래스에 의해 완성된다.

추상화
> 클래스간의 공통점을 찾아내서 공통의 조상을 만드는 작업
> - 상속 계층도가 위로 갈수록 공통요소만 남고, 밑으로 갈수록 세분화된다는 뜻