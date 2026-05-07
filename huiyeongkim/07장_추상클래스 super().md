## 1. 추상 클래스에서도 `super()`를 쓸까?

→ **쓴다 (필요하다)**

이유는 **객체 생성 과정에서 부모 부분을 먼저 초기화해야 하기 때문**이다.

자바에서 객체가 생성될 때 실제로는 **부모 → 자식 순서로 생성자 체인이 실행**된다.

```java
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
        System.out.println("Animal 생성자");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name); // 부모 생성자 호출
        System.out.println("Dog 생성자");
    }
}
```

```java
new Dog("choco");
```

```
// 실행 순서
Animal 생성자
Dog 생성자
```

<br>

## 2. 추상 클래스는 인스턴스를 만들지 않는데 왜 생성자가 필요할까?

**→ 추상 클래스 자체는 객체를 직접 생성하지 못하지만,
자식 객체 안에 포함되는 "부모 부분"은 반드시 생성되어야 한다.**

객체는 한 덩어리지만 내부 구조는 이렇게 나뉜다.

```
Dog 객체
 ├─ Animal 부분 (부모 필드)
 │    name
 │
 └─ Dog 부분 (자식 필드)
```

즉, **Dog 객체 안에는 Animal 객체 부분이 포함되어 있다.**

그래서 **Animal의 필드 초기화를 위해 생성자가 필요**하다.

<br>

## 3. "인스턴스는 하나만 생성된다"

```java
Dog dog = new Dog("choco");
```

객체는 하나만 생성된다.

```
[ Dog 객체 ]
    ├─ Animal 영역
    │     name = "choco"
    │
    └─ Dog 영역
```

즉, 실제 객체는 1개지만 메모리 내부에는 부모 클래스 부분과 자식 클래스 부분이 함께 존재한다.

<br>

## 4. 기본 클래스 vs 추상 클래스 

| 구분 | 일반 클래스 | 추상 클래스 |
| --- | --- | --- |
| 객체 생성 가능? | O | X |
| 생성자 존재? | O | O |
| super() 필요? | O | O |
| 부모 부분 초기화 필요? | O | O |

즉, 추상 클래스도 결국 **상속 구조에서 부모 역할을 하는 클래스**이기 때문에 생성자가 필요하다.
