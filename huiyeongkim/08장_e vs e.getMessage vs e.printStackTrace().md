# e vs e.getMessage vs e.printStackTrace()
## 1. e.getMessage()

```java
try {
    int[] arr = new int[5];
    arr[10] = 1;
} catch (Exception e) {
    System.out.println(e.getMessage());
}

// 출력 : Index 10 out of bounds for length 5
```

**예외 메시지 문자열만** 반환한다. 어디서 발생했는지 위치 정보는 없다.

## 2. e (toString())

```java
} catch (Exception e) {
    System.out.println(e);
}

// 출력 
// java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 5
```

이 호출되며 **예외 클래스명 + 메시지**를 출력한다.

## 3. e.printStackTrace()

```java
} catch (Exception e) {
    e.printStackTrace();
}
```

```java
java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 5
	at Main.method2(Main.java:15)
	at Main.method1(Main.java:10)
	at Main.main(Main.java:5)
```

**클래스명 + 메시지 + 전체 호출 스택**을 출력한다. 어떤 메서드를 거쳐서 예외가 발생했는지도 출력한다.

## 4. 요약

|  | 클래스명 | 메시지 | 스택 트레이스 |
| --- | --- | --- | --- |
| `e.getMessage()` | X | O | X |
| `e` (toString) | O | O | X |
| `e.printStackTrace()` | O | O | O |
