# 알아두면 쓸데있는 잡지식

<br>



## Serializable & Transient

> 데이터 전송을 위한 인터페이스

<br>



#### 1. Serializable

##### 1) 개념

>  데이터를 파일에 저장하거나, 네트워크에 실어보내기위해 바이트단위로 쪼개 늘어뜨리게 되는데
>
> 이를 __Serializable (직렬화)__ 라 한다.

- 기본 자료형은 별도의 처리를 거치치 않고도 직렬화 할 수 있다.
- 여러 자료형이 뒤섞여있는 클래스의 경우, __반드시! Serialized 인터페이스__ 를 구현해 주어야 한다.
- 직렬화된 클래스의 __Inner Class__ 또한 Serializable 인터페이스를 구현해야 한다.

<br>

##### 2) 예제

```java
public class DataClass implements Serializable {
  
  int id;
  String name;
  String address;
  Customer customer;
  
  public class Customer implements Serializable {
    int id;
    String storeName;
    ...
  }
}
```

<br>



#### 2. Transient

##### 1) 개념

> Serializable 을 구현할 때, 보안상의 이유등으로 직렬화 하고싶지 않은 필드(객체) 가 생길 수 있는데
>
> 이때 __Transient__ 를 선언해주게 되면, 원하는 필드(객체) 만 직렬화에서 제외 시킬 수 있다.

<br>

##### 2) 예제

```java
public class DataClass implements Serializable {
  
  int id;
  String name;
  transient String address;		// 특정 필드를 직렬화에서 제외
  transient Customer customer;	// 특정 Inner Class 를 직렬화에서 제외
  
  public class Customer {
    int id;
    String storeName;
    ...
  }
}
```

