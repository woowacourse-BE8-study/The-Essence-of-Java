> 자바에서 **Multi-threading**을 구현하기 위해 쓰레드를 생성하는 방법 2가지
>

# 1. Thread 클래스 상속 (`extends Thread`)

---

- `Thread` 클래스를 상속받아 새로운 자식 클래스를 정의하고, 그 안에서 `run()` 메서드를 **Overriding**하는 방식
- **구현 방법**:

    ```java
    class MyThread extends Thread {
        @Override
        public void run() {
            /* 쓰레드가 수행할 작업 내용 */
        }
    }
    ```

- **인스턴스 생성 및 실행**:

    ```java
    MyThread t1 = new MyThread();
    t1.start(); // 새로운 호출 스택(call stack)을 생성하고 run()을 호출함
    ```

- **특징**:
    - `Thread` 클래스의 메서드(예: `getName()`, `join()`, `sleep()`)를 클래스 내부에서 직접 호출할 수 있어 편리하다.
    - 클래스 자체가 쓰레드 객체가 되므로 사용법이 직관적이다.

# 2. Runnable 인터페이스 구현 (`implements Runnable`)

---

- `Runnable` 인터페이스를 구현하는 클래스를 정의하고 `run()` 메서드를 완성하는 방식이다.
- 실무와 객체지향 설계 관점에서는 **이 방법이 훨씬 일반적이고 권장된다.**
- **구현 방법**:

    ```java
    class MyThread implements Runnable {
        @Override
        public void run() {
            /* 쓰레드가 수행할 작업 내용 */
        }
    }
    ```

- **인스턴스 생성 및 실행**:

  `Runnable` 인터페이스는 `start()` 메서드가 없으므로, 이를 별도의 `Thread` 객체에 전달하여 실행해야 한다.

    ```java
    Runnable r = new MyThread();
    Thread t2 = new Thread(r); // 생성자 Thread(Runnable target) 이용
    t2.start();
    ```

- **특징**:
    - `Runnable`은 오로지 `run()` 메서드만 정의되어 있는 **Functional Interface**이다.
    - 작업 내용(`Runnable`)과 실행 메커니즘(`Thread`)을 분리하는 **Decoupling**이 가능하다.

# 3. 두 방법의 주요 차이점 및 장단점

---

| **구분**      | **Thread 상속**                           | **Runnable 구현**                                            |
|-------------|-----------------------------------------|------------------------------------------------------------|
| **상속의 제한**  | 자바는 다중 상속을 지원하지 않으므로 다른 클래스를 상속받을 수 없다. | 다른 클래스를 상속받으면서도 쓰레드 기능을 구현할 수 있어 유연하다.                     |
| **재사용성**    | 작업 내용과 쓰레드 제어 로직이 섞여 재사용성이 떨어진다.        | 동일한 `Runnable` 객체를 여러 쓰레드에 전달할 수 있어 **Reusability**가 높다.   |
| **객체지향 설계** | 쓰레드 그 자체를 확장하는 개념이다.                    | 수행할 '작업'만을 정의하는 개념으로, 더 객체지향적이다.                           |
| **메서드 접근**  | `getName()` 등을 직접 호출 가능하다.              | `Thread.currentThread().getName()`과 같이 정적 메서드를 통해 접근해야 한다. |

# 4. 더 알면 좋은 내용

---

### **1) 왜 Runnable이 더 객체지향적인가?**

객체지향 설계 원칙 중 하나인 **Single Responsibility Principle (단일 책임 원칙)** 관점에서 볼 때, `Thread` 클래스를 상속받는 것은 '쓰레드 실행' 기능과 '비즈니스 로직(작업
내용)'을 한 클래스에 몰아넣는 행위다. 반면 `Runnable`을 사용하면 '무엇을 할 것인가(Task)'와 '어떻게 실행할 것인가(Executor)'를 명확히 분리할 수 있다.

### **2) Single Source of Truth와 상태 공유**

`Runnable` 인터페이스를 구현하여 여러 개의 `Thread` 객체에 동일한 `Runnable` 인스턴스를 전달하면, 쓰레드들이 동일한 데이터(멤버 변수)를 공유하기 쉬워진다.

### **3) Modern Java에서의 활용**

Java 8 이후부터는 `Runnable`이 함수형 인터페이스가 됨에 따라, 별도의 클래스를 정의하지 않고도 **Lambda Expression**을 통해 매우 간결하게 쓰레드를 생성할 수 있다.

```java
Thread t = new Thread(() -> {
    System.out.println("Lambda Thread Start!");
});
t.

start();
```

### **4) start() vs run()**

- `run()`을 직접 호출하는 것은 단순히 클래스에 정의된 메서드를 실행하는 것일 뿐, 새로운 쓰레드가 생성되지 않는다.
- `start()`를 호출해야만 JVM이 새로운 **Call Stack**을 할당하고 쓰레드 스케줄러에 따라 독립적인 실행 흐름이 만들어진다.
