### 싱글톤 패턴 (Singleton Pattern)이란

- 클래스의 인스턴스를 단 하나만 생성하도록 보장함
- 그 인스턴스에 ‘어디서든 접근할 수 있는 전역적인 지점’을 제공하는 디자인 패턴

### 사용하는 이유

- **자원 절약:** 프로그램 내에서 인스턴스가 여러 개 있을 필요가 없는 경우 메모리 낭비를 방지
- **데이터 공유:** 여러 객체에서 하나의 공유된 상태(State)에 접근해야 할 때 유용함

### **Eager Initialization(이른 초기화)** 방식(기본 방식) 코드

```java
class Singleton {
    private static Singleton s = new Singleton();

    private Singleton() {
		...
    }

    public static Singleton getInstance() {
        return s;
    }
	...
}
```

**① `private` 생성자**

```java
private Singleton() { ...}
```

- **역할:** 외부에서 `new Singleton()`을 호출하지 못하게 막음
- **이유:** 생성자를 막아야 클래스 외부에서 인스턴스를 마음대로 찍어내는 것을 막을 수 있기 때문

**② `private static` 인스턴스 변수**

```java
private static Singleton s = new Singleton();
```

- **역할:** 클래스 내부에서 자기 자신 타입의 인스턴스를 미리 생성하여 들고 있음
- **이유:** `static` 영역에 올라가므로 프로그램 시작 시점에 딱 하나만 생성됨

**③ `public static getInstance()`**

```java
public static Singleton getInstance() {
    return s;
}
```

- **역할:** 유일하게 외부에서 인스턴스를 얻을 수 있는 통로
- **이유:** 인스턴스를 생성하지 않고도 접근해야 하므로 `static` 메서드로 정의함

### 상속과 `final`

- 생성자가 `private`이면 자식 클래스에서 조상의 생성자(`super()`)를 호출할 수 없어서 상속이 불가능해짐
- 클래스 선언부에 `final`을 붙여서 상속이 안 된다는 사실을 명확히 드러내는 게 좋음
  → 다른 개발자에게 ‘이 클래스는 확장이 불가능하며 의도적으로 설계된 것’이라는 메시지를 전달

### 다른 구현 방식들

1. **지연 초기화 (Lazy Initialization):** 인스턴스가 필요한 시점에 생성하여 초기 구동 속도를 높이는 방식
2. **멀티스레드 환경:** 여러 스레드가 동시에 `getInstance()`를 호출할 때 인스턴스가 2개 생기지 않도록 `synchronized`나 `Bill Pugh` 방식 등을 사용함
3. **Enum 싱글톤:** 자바 거장 조슈아 블로크(Joshua Bloch)가 권장하는, 직렬화와 리플렉션 공격까지 막아주는 가장 안전한 싱글톤 구현 방식

### 추가 학습하면 좋을 부분

- 단점 - 테스트의 어려움, 결합도 상승 등
