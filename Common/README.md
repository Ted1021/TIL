# 알아두면 쓸데있는 잡지식

<br>

## # Serializable & Transient

> 데이터 전송을 위한 인터페이스

#### 1. Serializable

##### 1) 개념

> 데이터를 파일에 저장하거나, 네트워크에 실어보내기위해 바이트단위로 쪼개 늘어뜨리게 되는데
>
>  이를 __Serializable (직렬화)__ 라 한다.

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

<br>

<br>

## # Runnable

> 특정 Thread 에서 원하는 실행로직이나, 변수들을 실어보내기 위한 클래스

#### 1. 정의

- __Runnable Interface__ 는 반드시 ___쓰레드___ 에 의해 실행 될 여지가 있는 클래스에 의해 구현되어야 한다.
- __Runnable Interface__ 는 실행될 여지가 있는 코드를 가진 객체에 대한 공통의 프로토콜을 가진다.
- 예를들어, Runnable 은 Thread 클래스에 의해 구현된다.
- 활성화가 된다는 것은 Thread 가 주욱 실행되어 왔고, 아직 끝나지 않았음을 의미한다.
- 게다가, Runnable 은 Thread 를 Subclassing 하지않고 활성화가 되는 클래스를 의미한다.
- Runnable 을 implements 한 클래스는 Thread 를 상속 받을필요가 없다. 
- 만약에 __run()__ 메소드 이에외 다른 Thread 클래스의 메소드를 override 할 필요가 없다면, 대부분의 케이스에서 Runnable interface 가 사용될 수 있다.
- 이는 매우 중요한데, 클래스들은 항상 상속될 필요가 없기 때문이다.