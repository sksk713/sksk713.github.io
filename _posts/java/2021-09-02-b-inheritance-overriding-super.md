---
layout: post
title: "02.상속/오버라이딩/super"
author: "SangKyenog Lee"
tags: Java
---

## 상속
> 기존의 클래스를 재사용하여 새로운 클래스를 작성한 것

- 보다 적은 양의 코드로 새로운 클래스를 작성할 수 있다.
- 코드의 추가 및 변경이 매우 용이하다.

> 재사용성을 높이고 코드의 중복을 제거하여 프로그램의 생산성과 유지보수에 크게 기여

`extends`키워드 사용
```java
class Child extends Parent {
    // ...
}
```
1. 조상클래스
> 부모 클래스, 상위 클래스

2. 자손 클래스
> 자식 클래스, 하위 클래스

```java
class Parent {
    int age;
}
class Child extends Parent {
    void play() {
        System.out.println("놀자");
    }
}
```

> 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속
> 자손 클래스의 멤버 개수는 조상 클래스보다 항상 같거나 많다.

### 상속 관계, 포함 관계
1. 원은 도형이다. - 상속
2. 원은 점을 가지고 있다. - 포함
```java
class Shape {
    String color = "black";
    void draw(){
        System.out.printf("%s",color);
    }
}

class point{
    int x;
    int y;
    
    Point(int x, int y){
        this.x = x;
        this.y = y;

    }
}

class Circle extends Shape{ // 원은 도형이므로 상속
    Point center; // 원은 point를 가지므로 포함
    int r;
}
```
### object클래스
> 모든 클래스의 조상클래스
- 컴파일러는 자동으로 object클래스로부터 상속 받도록 한다.
    - 이미 상속 받는 클래스가 있다면 따로 object클래스를 추가하지는 않는다.

## 오버라이딩
> 조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것

- 상속 받은 메서드를 보통은 그대로 사용하지만, 변경해야 할 경우 `조상의 메서드를 오버라이딩 한다`고 한다.

### 오버라이딩 조건
자손 클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와
1. 이름이 같아야 한다.
2. 매개변수가 같아야 한다.
3. 반환타입이 같아야 한다.

- 접근제어자와 예외
1. 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경 할 수 없다.
2. 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.
    - `Excetion`은 모든 예외의 최고 조상이므로 주의
3. 인스턴스메서드를 static메서드로 또는 그 반대로 변경할 수 없다.

### 오버라이딩 vs 오버로딩
- 오버라이딩
> 기존에 없는 새로운 메서드를 정의하는 것(new)

- 오버로딩
> 상속받은 메서드의 내용을 변경하는 것(change, modify)

```java
class Parent{
    void parentMethod() {}
}

class Child extends Parent {
    void parentMethod(){} // 오버라이딩
    void parentMethod(int i){} // 오버로딩
}
```
## super
> super는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 참조 변수

```java
class Parent {
    int x = 10;
}
class Child extends Parent {
    int x = 20;
    void method(){
        System.out.println(this.x); // 20
        System.out.println(super.x); // 10
    }
}
```

### super()
> this()와 같은 생성자
> 조상 클래스의 생성자를 호출하는데 사용

> Object클래스를 제외한 모든 클래스의 생성자 첫 줄에 생성자 this() 또는 super()를 호출해야 한다. 그렇지 않으면 컴파일러가 자동적으로 super()를 생성자의 첫 줄에 삽입

```java
class Point3D extends Point{
    int x;

    Point3D(int x, int y, int z){
        // 여기서 다른 생성자를 호출하지 않아서 super()를 호출 한다.
        // super()는 조상 클래스인 Point 클래스의 기본 생성자 Point()를 의미

        this.x = x;
        this.y = y;
        this.z = z;
    }
}
```