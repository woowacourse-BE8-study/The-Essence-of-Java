## 1. Call by Value (값에 의한 호출)

함수(메서드)에 **값의 복사본을 전달**하는 방식이다.
→ 원본 값은 변경되지 않는다.

```java
public class Main {
    public static void main(String[] args) {
        int num = 10;
        changeValue(num);

        System.out.println(num); // 10
    }

    static void changeValue(int num) {
        num = 20;
    }
}
```


```
// 메모리 관점
main stack
num = 10

changeValue stack
num = 10 (복사본)
```

changeValue 안에서 값을 바꿔도 **복사본만 변경된다.**

<br>

## 2. Call by Reference (참조에 의한 호출)

함수에 **원본 변수 자체를 전달한다.**
→ 함수 안에서 값을 변경하면 원본도 변경된다.

```cpp
// C++ 예시 (참고)
void changeValue(int &num) {
    num = 20;
}
```

Java에는 이런 방식이 **존재하지 않는다.**

<br>

## 3. 그런데 왜 Java는 Call by Reference처럼 보일까?

```java
class User {
    String name;
}

public class Main {
    public static void main(String[] args) {
        User user = new User();
        user.name = "kim";

        changeName(user);

        System.out.println(user.name); // lee
    }

    static void changeName(User user) {
        user.name = "lee";
    }
}
```

```
// 결과
lee
```

왜 바뀌었을까요?

<br>

## 4. 객체 전달 시 실제 동작

객체 자체가 전달되는 것이 아니라 **객체의 참조값(reference 값)이 복사되어 전달된다.**

```
// 메모리 구조
Heap
User 객체
name = "kim"

Stack (main)
user → 0x1234

Stack (changeName)
user → 0x1234 (복사된 참조값)

main의 user와 changeName의 user는 서로 다른 변수이다.
하지만 그 변수 안에 들어있는 값(참조값)이 동일해서 같은 Heap 객체를 가리킨다
```

둘 다 **같은 객체를 가리키고 있다.**


```
// 결과
user.name="lee";
```

→ 같은 객체를 수정하기 때문에 값이 변경된다.

<br>

## 5. 진짜 Call by Reference였다면?

참조 자체를 바꾸는 것도 영향을 줘야 한다.

```java
static void changeUser(User user) {
    user = new User();
    user.name = "park";
}
```

실행:

```java
User user = new User();
user.name = "kim";

changeUser(user);

System.out.println(user.name);
```

```
// 결과
kim
```

```
// 메모리 관점
main stack
user → 0x1234

changeUser stack
user → 0x5678 (새 객체)
```

참조값 자체를 바꾸면 원본에는 영향 없다. → 그렇기 때문에 Call by Value이다.

<br>

## 6. 정리

| 구분 | primitive | 객체 |
| --- | --- | --- |
| 전달 값 | 실제 값 복사 | 참조값 복사 |
| 메서드에서 값 변경 | 원본 영향 X | 원본 영향 O (객체 내부 값 변경 시) |
| 참조 자체 변경 | 해당 없음 | 원본 영향 X |
| Java 방식 | Call by Value | Call by Value |

자바는 항상 call by value 이다!
