---
layout: post
title: "[자바의 정석] 인터페이스/내부클래스"
author: "SangKyenog Lee"
tags: Java
---

< Java의 정석 >을 공부하고 정리 했습니다. 

## 인터페이스
> - 인터페이스는 일종의 추상 클래스라고 할 수 있다.
> - 인터페이스는 추상클래스처럼 추상메서드를 갖지만 추상클래스보다 추상화 정도가 높아서 추상클래스와 달리 몸통을 갖춘 일반 메서드 또는 멤버변수를 구성원으로 가질 수 없다.
> - 오직 추상메서드와 상수만을 멤버로 가질 수 있으며, 그 외의 다른 어떠한 요소도 허용하지 않는다.

- 추상클래스를 `미완성 설계도`라고 한다면 인터페이스는 밑그림만 그려져 있는 `기본 설계도`라고 할 수 있다.

### 인터페이스 작성
1. 키워드로 `class`대신 `interface`를 사용한다.
2. interface에 접근제어자로 `public` 또는 `default`를 사용할 수 있다.
3. 모든 멤버변수는 `public static final`이어야 하며, 생략 가능
4. 모든 메서드는 `public abstract`이어야 하며, 생략 가능
    - static메서드와 default메서드는 예외(jdk1.8부터)
```java
interface 인터페이스이름 {
    public static final 타입 상수이름 = 값;
    publci abstract 메서드이름 (매개변수목록);
}
```
### 인터페이스 상속
> - 인터페이스는 인터페이스로부터만 상속받을 수 있다.
> - 클래스와는 달리 다중상속이 가능하다.

```java
interface Movable{
    void move(int x, int y);
}
interface Attackable{
    void attack(Unit u);
}
interface Fightable extends Movable, Attackable {}
```

### 인터페이스 구현
> - 인터페이스도 추상클래스처럼 그 자체로 인스턴스 생성불가
> - `implements`라는 키워드로 인터페이스를 클래스에 구현

```java
class 클래스이름 implements 인터페이스이름 {
    // 인터페이스에 정의된 추상메서드 구현
}
class Fighter implements Fightable {
    public void move(int x, int y){}
    public void attack(Unit u){}
}
```
- 위를 `Fighter클래스는 Fightable인터페이스를 구현한다`라고 한다.

- 전체 예제
```java
class FighterTest {
	public static void main(String[] args) {
		Fighter f = new Fighter();

		if (f instanceof Unit)	{		
			System.out.println("f는 Unit클래스의 자손입니다.");
		}
		if (f instanceof Fightable) {	
			System.out.println("f는 Fightable인터페이스를 구현했습니다.");
		}
		if (f instanceof Movable) {		
			System.out.println("f는 Movable인터페이스를 구현했습니다.f");
		}
		if (f instanceof Attackable) {	
			System.out.println("f는 Attackable인터페이스를 구현했습니다.");
		}
		if (f instanceof Object) {		
			System.out.println("f는 Object클래스의 자손입니다.");
		}
	}
}

class Fighter extends Unit implements Fightable {
	public void move(int x, int y) {}
	public void attack(Unit u) {}
}

class Unit {
	int currentHP;	
	int x;			
	int y;		
}

interface Fightable extends Movable, Attackable { }
interface Movable {	void move(int x, int y);	}
interface Attackable {	void attack(Unit u); }
```

- Movable 인터페이스에서 `void move`가 정의되어 있지만 `public abstract`가 생략된 것이기 때문에 이를 구현하는 Fighter클래스에서는 `public`을 `void move`의 접근제어자로 사용해야 한다.

### 인터페이스 다중상속, 다형성
> 자바에서는 상속을 받을 때 멤버변수의 이름이 같거나 메서드의 선언부가 일치하고 구현 내용이 다르면 어느 조상의 것을 상속받는지 모르기 떄문에 다중상속을 허용하지 않는다.

- 크게 쓸 일은 없으므로 나중에 추가적으로 공부

### 인터페이스 장점
1. 개발시간을 단축시킬 수 있다.
2. 표준화가 가능하다.
3. 서로 관계없는 클래스에게 관계를 맺어 줄 수 있다.
4. 독립적인 프로그래밍이 가능하다.

### 인터페이스 이해
- 인터페이스의 본질은 다음 두 사항을 알아야 한다.
    1. 클래스를 사용하는 쪽(User)과 클래스를 제공하는 쪽(Provider)이 있다.
    2. 메서드를 사용하는 쪽에서는 사용하려는 메서드의 선언부(이름)만 알면 된다.

```java
 class A {
    void autoPlay(I i) {
          i.play();
     }
 }

 interface I {
      public abstract void play();
 }

 class B implements I {
     public void play() {
          System.out.println("play in B class");
     }
 }

 class C implements I {
     public void play() {
          System.out.println("play in C class");
     }
 }

class InterfaceTest2 {
	public static void main(String[] args) {
		A a = new A();
		a.autoPlay(new B());
		a.autoPlay(new C());
	}
}
```
- 다음 예제처럼 클래스 B의 선언과 구현을 분리해서 두 클래스간의 관계를 간접적으로 변경해야 한다.
- A가 클래스 B를 직접 호출하지 않고 인터페이스를 매개체로 하여 클래스 A가 인터페이스를 통해 클래스 B의 메서드에 접근한다.
- 그렇게 되면 클래스 B의 변경사항에 클래스 A는 직접적으로 영향을 받지 않는다.

### 인터페이스와 static, default 메서드
- static
> - jdk1.8이전에는 추상 메서드만 선언 가능했지만 1.8부터 디폴트와 static 메서드를 추가할 수 있다.
> - static은 인스턴스와 관계 없이 독립적인 메서드이기 때문에 영향을 주지 않기 떄문
- default
> - 인터페이스에 새로운 메소드를 구현하면, 이 인터페이스를 사용하는 모든 클래스들이 새로운 메서드를 구현해야 한다.
> - 이러한 문제때문에 인터페이스에서 다른 기존 모든 클래스에 영향을 주지 않기 위해 default 메서드를 추가하였다.

#### 충돌 시
1. 여러 인터페이스의 디폴트 메서드 간의 충돌
    - 인터페이스를 구현한 클래스에서 디폴트 메서드를 오버라이딩 한다.
2. 디폴트 메서드와 조상클래스의 메서드 간의 충돌
    - 조상 클래스의 메서드가 상속되고, 디폴트 메서드는 무시된다.

### 내부클래스
- 추후 공부