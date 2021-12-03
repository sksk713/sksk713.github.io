---
layout: post
title: "[자바의 정석] 지네릭스(Generics)/열거형(Enum)/애너테이션(annotation)"
author: "SangKyenog Lee"
tags: Java
---

< Java의 정석 >을 공부하고 정리 했습니다.

# 지네릭스
지네릭스는 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스들의 타입 체크를 컴파일 시에 해주는 기능이다. 컴파일 시에 타입체크를 함으로써, 객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.
- 타입 안정성을 높이는 것은 의도치 않은 타입의 저장을 막고, 객체를 꺼내올때 타입으로 인한 오류가 줄어드는 것을 말한다.

(example)

```java
class Box {
    Obeject item;

    void setItem(Object item) {
        this.item = item;
    }
    Object getItem() {
        return item;
    }
}
// 지네릭 클래스로 변경
class Box<T> {
    T item;

    void setItem(T item) {
        this.item = item;
    }
    T getItem() {
        return item;
    }
}
```
위 코드에서 `<T>`의 T를 타입 변수라고 하며 의미있는 문자를 선택해도 되고, 기존에 있는 타입으로 선언해도 된다.

## 용어 정리
```java
Box<T> // 지네릭 클래스. 'T의 Box' 또는 'T Box'라고 읽는다.
T // 타입 변수 또는 타입 매개변수
Box // 원시 타입
```

## 지네릭스의 제한
```java
Box<Apple> appleBox = new Box<Apple>();
Box<Grape> grapeBox = new Box<Grape>();
```
위의 코드처럼 객체별로 다른 타입을 지정하는 것은 적절하다고 할 수 있다.

그러나 모든 객체에 동일하게 동작해야 하는 static멤버에 타입 변수 T를 사용할 수 없다. T는 인스턴스 변수로 간주되는데 static멤버는 인스턴스변수를 참조할 수 없기 때문이다.<br>
그리고 지네릭 타입의 배열을 생성하는 것도 허용되지 않는다. `new`연산자를 사용하게 되면 컴파일 시에 타입 T가 뭔지 정확하게 알아야 되는데 컴파일 시에 알 수 없고, `instanceof`연산자 또한 같은 이유로 T를 피연산자로 사용할 수 없다.
- 배열을 사용하기 위해서는 newInstance() API나 Object배열을 생성한 후에, 복사하고 형변환하는 방법 등이 있다.

`<T>`타입을 String, Integer처럼 지정하는 것 말고 조금 더 범위를 넓혀서 제한하는 법이 존재하는데 바로 상속을 이용하는 것이다.
```java
class FruitBox<T extends Fruit>
```

## 와일드 카드

```java
class Juicer {
    static Juice makeJuice(FruitBox<Fruit> box) {
        String tmp = "";
        for (Fruit f : box.getList()) {
            tmp += f + " ";
        }
        return new Juice(tmp);
    }
}
```
위의 코드에서 우리가 만약

```java
Juicer.makeJuice(fruitBox)); // <Fruit>
Juicer.makeJuice(appleBox)); // <Apple>
```
이 두개의 메서드를 호출 한다고 했을때 위에서 Fruit으로 이미 정해놨기 때문에 다른 타입인 Apple을 호출하게 되면 에러가 발생한다. 이런 상황에서 우리는 static메서드를 두개 만들어서 `오버로딩`을 시도한다고 해도 에러는 사라지지 않는다. 왜냐하면 지네릭은 타입이 다른 것만으로는 오버로딩이 성립되지 않기 때문이다. 그 이유는 `컴파일 시에만 지네릭 타입을 파악하고 지워버리기 때문에 두 메서드는 따지고 보면 메서드 중복 정의`에 해당한다.

이런 상황에서 이것을 해결하기 위해 `와일드 카드`가 존재한다.<br>
```java
<? extends T> // 와일드 카드의 상한 제한, T와 그 자손들만 가능
<? super T> // 와일드 카드의 하한 제한, T와 그 조상들만 가능
<?> // 제한 없음, 모든 타입 가능
```

결과적으로, 와일드 카드를 도입해서 위의 makeJuice메서드의 매개변수를

```java
static Juice makeJuice(FruitBox<? extend Fruit> box)
```
으로 수정하면 Fruit뿐만 아니라 Apple, Grape 등도 가능해진다.

## 형변환

원시 타입 - 지네릭 타입간에는 형변환이 가능하지만 경고 발생

지네릭 타입 - 지네릭 타입간에는 형변환이 불가능하다.
- 예) `Box<Integer> Box<String>`

와일드 카드 - 지네릭 타입간에는 형변환이 가능하다.
- 예) `Box<? extends Object> wBox = new Box<String>()`

## 지네릭 제거
컴파일러는 지네릭 타입을 사용해 소스파일을 체크하고 필요한 곳에 형변환을 해준다. `그 후에는 지네릭 타입을 삭제한다.` 즉 컴파일 된 파일에는 지네릭 타입에 대한 정보가 없다는 뜻이다.
- 지네릭 이전 코드와의 호환을 위해
<br>

제거 순서
1. 지네릭 타입의 경계를 제거한다
    - 만약 `<T extends Fruit>이라면 T는 Fruit으로 치환된다.`
2. 지네릭 타입을 제거한 후에, 타입이 일치하지 않으면 형변환을 추가한다.