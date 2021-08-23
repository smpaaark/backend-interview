## Java
* [Call by value와 Call by reference](https://github.com/smpark1020/backend-interview/tree/master/Java#call-by-value%EC%99%80-call-by-reference)
* [상속(Inheritance)과 구성(Composition)]()
* [접근제어자]()
* [Java8의 인터페이스]()
* [익명 클래스와 람다]()
* [String과 new String]()
* [Immutable Class(불변 클래스)]()
* [Collection 프레임워크]()
* [싱글톤 동시성 문제 해결 방법]()

[뒤로](https://github.com/smpark1020/backend-interview#back-end-interview)

## Call by value와 Call by reference
### Call by value
* 값에 의한 호출
* 메서드 호출 시에 사용한 인자와 메서드 내의 매개변수는 서로 다릅니다.
* 메서드 호출 시에 사용한 인자는 할당된 메모리 주소가 아닌 메모리에 담겨져 있던 값만이 복사되어 메서드 내부 매개변수의 메모리 주소에 담겨지게 됩니다.
* 메서드 내에서 값을 변경하더라도 호출 시에 사용한 인자는 변하지 않습니다.
* 요점
```
Call by value는 메서드 호출 시에 사용되는 인자의 메모리에 저장되어 있는 값(value)을 복사하여 보냅니다.
```

### Call by reference
* 참조에 의한 호출
* 메서드 호출 시에 사용되는 인자가 값이 아닌 주소(Address)를 넘겨줌으로써, 주소를 참조(Reference)하여 데이터를 변경할 수 있습니다.
* 메서드 호출 시에 인자는 메모리에 저장된 주소 값을 복사하여 매개변수의 메모리에 저장합니다.
* 결국 메서드는 호출 시 인자의 주소를 참조하여 연산하기 때문에, 연산 결과에 따라 원본 데이터가 변하게 됩니다.
* 요점
```
Call by reference는 메서드 호출 시 사용되는 인자 값의 메모리에 저장되어있는 주소(Address)를 복사하여 보낸다.
```

### 참조
* [[Java] Call by value와 Call by reference](https://re-build.tistory.com/3)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## 상속(Inheritance)과 구성(Composition)
### 상속을 사용하는 이유
* 공통적인 부분을 가지고 있는 상위 클래스를 활용하여 하위 클래스는 본인 고유의 상태와 행동을 정의하기 위함입니다.
* 코드의 확장성, 재사용성이 용이하고 중복된 코드를 상위 클래스로 뺐으므로 코드가 간결해집니다.
* 결과적으로 유지보수가 쉬워진다는 장점이 있습니다.

### 상속 정의
하위 클래스는 상위 클래스의 모든 메소드를 재사용할 수 있고, 재정의를 하여 하위 클래스만의 메소드로 변경할 수도 있습니다.

### 상속의 단점
* 상속은 단일 패키지에서 사용해야만 안전합니다.
* 하위 클래스는 상위 클래스에 많이 의존하게 됩니다.
* 상위 클래스의 코드가 수정되면 하위 클래스의 코드도 수정되어야 하는 경우가 많습니다. (재정의 메소드)
* 확장이라는 목표를 두고 상속을 사용하면 괜찮으나, 해당 설계가 아닌 상황에서 상속을 사용할 시 문제를 발생시킬 수 있습니다.
* 상속은 캡슐화를 위반합니다.
  * 하위 클래스가 상위 클래스의 구체적인 구현 내용을 의존하고 있기 때문입니다.

### 구성(Composition)
* Composition은 하위 클래스가 상위 클래스를 상속받는 것이 아니라 단순히 하위 클래스가 상위 클래스를 private로 참조받아서 사용하는 것입니다.
* 그 후 필요한 메소드를 가져다 사용하는 것입니다. (포워딩 - wrapper class)

### 정리
* 상속은 상위 클래스와 하위 클래스가 순수한 is-a 관계일 때만 사용해야 합니다.
* is-a 관계라고해서 무조건 사용하는 것이 아니라, 같은 패키지에 있어야 하며 상위 클래스가 확장을 고려하여 설계되어 있어야 합니다.
* 상속의 취약점을 피하고 싶다면 Composition을 사용해야 합니다.
* 구성은 has-a 관계입니다.

### 참조
* [[Java 기초] 상속과 구성](https://it-mesung.tistory.com/39)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## 접근제어자
* private -> default -> protected -> public 순으로 보다 많은 접근을 허용합니다.
* 프로그램의 위험요소를 제거할 수 있습니다.

### private
* 변수, 메소드는 해당 클래스에서만 접근이 가능합니다.

### default
* 접근제어자를 별도로 설정하지 않는다면 접근제어자가 없는 변수, 메소드는 default 접근제어자가 되어 해당 패키지 내에서만 접근이 가능합니다.

### protected
* 변수, 메소드는 동일 패키지내의 클래스 또는 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근이 가능합니다.

### public
* 변수, 메소드는 어떤 클래스에서라도 접근이 가능합니다.

### 참조
* [07-2 접근제어자 (Access Modifier)](https://wikidocs.net/232)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## Java8의 인터페이스
### 인터페이스가 가질 수 있는 것들
* 상수 필드(public static final)
* 추상 메서드(public abstract)
* 디폴트 메서드(public default)
  * 실행 내용까지 작성이 가능합니다.
  * default 키워드를 이용하여 작성한 메서드는 내용을 작성 가능하며 override 할 수 있습니다.
  ```
  public interface Parent{     

    public default void setState(boolean state){
        if(state){
            System.out.println("현재 상태는 정상입니다");
        }else{
            System.out.println("현재 상태는 비정상입니다");
        }
    }

  }
  ```
* 정적 메서드(public static)
  * Parent.change() 식으로 객체 없이 인터페이스만으로도 호출이 가능합니다.
  * override가 불가능합니다.
  ```
  public interface Parent{   

    public static void change(){
        System.out.println("상태를 변경합니다.");
    }

  }
  ``` 

### 기타 특징
* 익명 구현 객체
  * Java 8에서 지원하는 람다식은 인터페이스의 익명 구현 객체를 생성할 수 있도록 추가되었습니다.
  * UI 프로그래밍에서 이벤트를 처리하거나, 임시 작업 스레드를 만들기 위해서 유용하게 사용될 수 있습니다.
* 자동 타입 변환
* 강제 타입 변환
  * 구현 객체가 인터페이스 타입으로 자동 타입 변환되는 경우, 인터페이스에 선언된 메소드만 사용이 가능합니다.
  * 만약에 구현 클래스에 선언된 필드와 메서드를 사용해야 하는 경우에는 강제 타입 변환을 이용하여 다시 구현 클래스 타입으로 변환해야 합니다.
* Functional Interface
  * 람다식이나 메소드 레퍼런스를 위한 할당 대상으로 사용될 수 있습니다.
  * Object 클래스의 메소드를 제외하고 구현해야 할 추상 메서드가 하나만 정의된 인터페이스를 가리킵니다.
    * Object 객체의 메소드만 인터페이스에 선언되어 있는 경우는 Functional Interface가 아님
    * Object 객체의 메소드를 제외하고 하나의 추상 메소드만 선언되어 있는 경우는 Functional Interface 임
    * Object 객체의 clone 메소드는 public 메소드가 아니기 때문에 Functional Interface의 대상이 됨
  * Java 8에서는 @FuctionalInterface라는 어노테이션을 제공하여 작성한 인터페이스가 Functional Interface인지 확인할 수 있도록 하고 있습니다.
    * 이것을 사용하면 부적절한 메소드 선언이 포함되어 있거나 함수형이어야 하는 인터페이스가 다른 인터페이스를 상속한 경우를 미리 확인할 수 있습니다.
    * Functinal Interface가 아닐 경우 컴파일 에러가 발생합니다.

### 정리
* interface의 default 메소드
  * interface에서도 메소드 구현이 가능합니다.
  * 참조 변수로 함수를 호출할 수 있습니다.
  * implements 한 클래스에서 재정의가 가능합니다.
* interface의 static 메소드
  * interface에서 메소드 구현이 가능합니다.
  * 반드시 클래스 명으로 메소드를 호출해야 합니다.
  * 재정의가 불가능합니다.
* 익명 객체를 구현할 수 있습니다.
* Functional 인터페이스를 이용하면 람다를 이용하여 인스턴스를 생성할 수 있습니다.

### 참조
* [Java 8 - Interface바뀐점을 알아보기](https://beomseok95.tistory.com/272)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## 익명 클래스와 람다
인터페이스는 직접 객체화할 수 없기 때문에 구현 클래스를 이용하는데 일회성으로 사용하는 구현 클래스를 계속 선언하는 것은 비효율적이기 때문에 익명 클래스나 람다를 이용하여 구현 클래스를 선언합니다.

### 익명 클래스
* 익명 클래스는 코드가 너무 길어 함수형 프로그래밍 방식에 적합하지 않습니다.
* 익명 클래스는 말그대로 이름이 없는 클래스이며 new와 동시에 부모클래스를 상속받아 내부에서 오버라이딩해서 사용합니다.
* 매개변수로도 사용이 가능하며 상속은 받아야하지만 일회성으로 사용할 때 주로 사용합니다.
* 익명 클래스는 외부의 자원을 이용할 때 final이 붙은 자원만 사용 가능합니다.

### 함수 객체(Function Object)
특정 동작을 목적으로 추상 메서드를 하나만 담은 인터페이스나 추상 클래스를 함수 객체라합니다.

### 람다
JDK 1.8 이후로는 함수 객체의 인스턴스를 람다식으로 만들어 사용할 수 있습니다.

### 공통점
* 익명클래스나 람다가 선언되어 있는 바깥 클래스의 멤버 변수나 메서드에 접근할 수 있습니다.
* 하지만 멤버 변수나 메서드의 매개변수에 접근하기 위해서는 해당 변수들이 final의 특성을 가지고 있어야 합니다.
* 이 말은 final로 선언하지 않았더라도 변수를 변경하지 않으면 실질적으로 final과 같기 때문에 final을 생략할 수 있음을 말합니다.

### 차이점
1. 익명클래스와 람다식에서의 this의 의미는 다릅니다.
  * 익명클래스의 this는 익명클래스 자신을 가리키지만 람다에서의 this는 선언된 클래스를 가리킵니다.
2. 람다는 은닉 변수(Shadow Variable)을 허용하지 않습니다.
  * 익명 클래스는 변수를 선언하여 사용할 수 있지만 람다는 이를 허용하지 않습니다.
3. 람다는 인터페이스에 반드시 하나의 메서드만 가지고 있어야 합니다.
  * 인터페이스에 @FunctionalInterface 어노테이션을 붙이면 두 개 이상의 추상 메서드가 선언되었을 경우 컴파일 에러를 발생시킵니다.
4. 람다는 같은 시그니처를 가지는 인터페이스에 대하여 의미가 모호해집니다.

### 참조
* [익명클래스 vs 람다](https://skasha.tistory.com/34)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## String과 new String
* Java에서 String은 특별한 참조 자료형입니다.
* String 객체는 자바에 내장된 클래스로 new 키워드로 새로운 객체를 생성할 수도 있고, ""안에 값을 입력하여 생성할 수도 있습니다.
* Java의 문자열은 java.lang 패키지의 String 클래스의 인스턴스로 관리됩니다. 
* 소스상에서 문자열 리터럴은 String 객체로 자동생성되지만, String 클래스의 다양한 생성자를 이용해서 직접 String 객체를 생성해서 사용할 수도 있다.
* 두 가지 방식 모두 String 객체를 생성한다는 사실은 같지만, JVM이 관리하는 메모리 구조상에서 명백히 다릅니다.
* new 생성자를 이용해서 인스턴스를 생성한 뒤, heap에서 메모리 관리가 이루어진다는 사실은 다른 참조 자료형과 다를게 없습니다.
* 하지만 다른 참조형과는 다르게 변하지 않는다는 특징을 갖고 있습니다.
* 한번 저장된 String 객체의 값은 변하지 않습니다.
* String 객체들의 연산이 이루어지면, 새로운 객체를 계속 만들어내기 때문에 메모리 관리 측면에서 상당히 비효율적입니다.
* 이러한 이유로 만들어진 메모리영역이 Heap 안에 있는 String Constant Pool 입니다.
* 여기에는 기존에 만들어진 문자열 값이 저장되어 있고, 리터럴로 생성된 같은 값을 가지는 객체는 같은 레퍼런스를 가지게 됩니다.

### 참조
* [[JAVA] String = " " vs new String(" ") 의 차이](https://ict-nroo.tistory.com/18)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## Immutable Class(불변 클래스)
* 변경이 불가능한 클래스이며, 가변적이지 않는 클래스입니다.
* 레퍼런스 타입의 객체이기 때문에 heap 영역에 생성됩니다.
* heap 영역에서 변경불가능한 것이지 재할당을 못하는 것은 아닙니다.
  * 재할당은 heap 영역의 객체가 바뀌는 것이지 heap 영역에 있는 값이 바뀌는 것이 아닙니다.

### 대표 예시
* String, Boolean, Integer, Float, Long 등등

### Immutable의 특징
* 장점
  * 생성자, 접근메소드에 대한 방어 복사가 필요없습니다.
  * 멀티스레드 환경에서 동기화 처리없이 객체를 공유할 수 있습니다.
  * Thread-safe
  * 불변이기 때문에 객체가 안전합니다.
* 단점
  * 객체가 가지는 값마다 새로운 객체가 필요합니다.
  * 따라서 메모리 누수와 새로운 객체를 계속 생성해야하기 때문에 성능저하를 발생시킬 수 있습니다.

### 대표적인 Immutable인 String
* String은 값을 변화시키는 것이 아니라 String 내부적으로 아예 새로운 String 객체를 만듭니다.
* 즉 문자열을 변경하는 작업을 하는 경우 새롭게 객체를 만들어서 리턴합니다.
* String 내부의 모든 메서드들의 return 값은 모두 new String() 입니다.
* String을 계속 변화시키면 계속 객체를 만들기 때문에 성능저하 문제가 발생할 수 있습니다.

### Immutable Class 만드는 방법
1. 멤버 변수를 final로 선언
2. 접근 메서드를 구현하지 않는다. (Setter 메소드)
3. mutable한 멤버변수 타입일 경우 생성자에서 새로 객체를 생성해서 필드값을 초기화해줍니다.

멤버 변수가 final로 선언되었기 때문에 수정이 불가하고, 또, Setter 접근 메소드가 없기 때문에 기본적으로 변경이 불가능합니다.

### 참조
* [[Java] Immutable Class (불변 클래스)](https://limkydev.tistory.com/68)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## Collection 프레임워크
* 컬렉션이란 데이터의 집합, 그룹을 의미하며 컬렉션 프레임워크는 이를 구현하는 클래스를 정의하는 인터페이스를 제공합니다.
* Collection 인터페이스는 List, Set, Queue로 크게 3가지 상위 인터페이스로 분류할 수 있습니다.
* Map의 경우 Collection 인터페이스를 상속 받고 있지 않지만 Collection으로 분류됩니다.

### Set 인터페이스
순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않습니다.
* HashSet
  * 가장 빠른 임의 접근 속도
  * 순서를 예측할 수 없음
* TreeSet
  * 정렬 방법을 지정할 수 있음

### List 인터페이스
순서가 있는 데이터의 집합으로 데이터의 중복을 허용합니다.
* LinkedList
  * 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용
  * 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임
* Vector
  * 과거에 대용량 처리를 위해 사용했으며, 내부에서 자동을 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음
* ArrayList
  * 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남

### Map 인터페이스
키(Key), 값(Value)의 쌍으로 이루어진 데이터 집합으로, 순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.
* Hashtable
  * HashMap보다는 느리지만 동기화 지원
  * null 불가
* HashMap
  * 중복과 순서가 허용되지 않으며 null값이 올 수 있다.
* TreeMap
  * 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠름

### 참조
* [[JAVA] Java 컬렉션(Collection) 정리](https://gangnam-americano.tistory.com/41)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)

## 싱글톤 동시성 문제 해결 방법
### Java에서 Singleton 패턴이란?
* 어떤 클래스가 최초 한번만 메모리를 할당하고(static) 그 메모리에 객체를 만들어 사용하는 디자인 패턴을 의미합니다.
* 즉 생성자의 호출이 반복적으로 이뤄져도 실제로 생성되는 객체는 최초 생성된 객체를 반환해주는 것입니다.
* 변수를 static을 줌으로써 인스턴스화 하지 않고 사용할 수 있게 하였지만 접근 제한자가 private으로 되어 있어 직접적인 접근은 불가능합니다.
* 또한 생성자도 private으로 되어 있어 new를 통한 객체 생성도 불가능합니다.
* 결국 getInstance() 메소드를 통해서 해당 인스턴스를 얻을 수 있게 됩니다.

### 싱글톤 패턴을 사용하는 이유
* 한번의 객체 생성으로 재사용이 가능하기 때문에 메모리 낭비를 방지할 수 있습니다.
* 무조건 한번 생성으로 전역성을 띄기에 다른 객체와 공유가 용이합니다.

### 싱글톤의 문제점
* 싱글톤으로 만든 객체의 역할이 간단한 것이 아닌 복잡한 경우라면 해당 싱글톤 객체를 사용하는 다른 객체간의 결합도가 높아져서 객체 지향 설계 원칙에 어긋나게 됩니다. (개방-폐쇄)
* 또한 해당 싱글톤 객체를 수정할 경우 싱글톤 객체를 사용하는 곳에도 사이드 이팩트 발생 확률이 생기게 되며, 멀티 스레드 환경에서 동기화 처리 문제등이 생기게 됩니다.

### 다양한 싱글톤의 구현
* static block
  * static 블럭을 사용할 경우 클래스가 로딩될 때 한번만 실행을 하게 되는 특성을 사용합니다.
  * 하지만 인스턴스가 사용되는 시점이 아닌 클래스 로딩 시점에 실행이 됩니다.
* lazy init
  * 클래스 로딩 시점이 아닌 인스턴스가 필요하여 요청할 때 생성되는 형태로 작성됩니다.
  * 하지만 멀티 스레드 환경에서 취약합니다.
  * 특정 스레드가 동시에 getInstance() 메소드를 호출하게 되면 인스턴스가 두 번 생성되는 문제가 발생합니다.
* Thread safe + lazy
  * getInstance() 메소드에 synchronized 키워드를 붙임으로써 스레드에서 동시 접근에 대한 문제를 해결합니다.
  * 하지만 synchronized 키워드는 성능 저하를 발생시킵니다.
* Holder
  * JVM의 클래스 로더 메커니즘과 클래스의 로드 시점을 이용하여 내부 클래스를 통해 생성 시킴으로써 스레드 간의 동기화 문제를 해결합니다.
  * 이 방법은 현재 java에서 싱글톤 객체 생성에 사용하는 대표적인 방법입니다.

### 참조
* [Java에서 싱글톤(Singleton) 패턴을 사용하는 이유와 주의할 점](https://elfinlas.github.io/2019/09/23/java-singleton/)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Java#java)