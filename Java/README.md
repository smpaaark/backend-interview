## Call by value와 Call by reference
### Call by value
* 값에 의한 호출
* 메서드 호출 시에 사용한 인자는 할당된 메모리 주소가 아닌 메모리에 담겨져 있던 값만이 복사되어 메서드 내부 매개변수의 메모리 주소에 담겨지게 됩니다.
* 메서드 내에서 값을 변경하더라도 호출 시에 사용한 인자는 변하지 않습니다.

### Call by reference
* 참조에 의한 호출
* 메서드 호출 시에 사용되는 인자가 값이 아닌 주소(Address)를 넘겨줌으로써, 주소를 참조(Reference)하여 데이터를 변경할 수 있습니다.
* 결국 메서드는 호출 시 인자의 주소를 참조하여 연산하기 때문에, 연산 결과에 따라 원본 데이터가 변하게 됩니다.

## 상속(Inheritance)과 구성(Composition)
### 상속
* 공통적인 부분을 가지고 있는 상위 클래스를 활용하여 하위 클래스는 본인 고유의 상태와 행동을 정의하는 것입니다.
* 코드의 확장성, 재사용성이 용이하고 중복된 코드를 상위 클래스로 뺐으므로 코드가 간결해집니다.
* 상위 클래스의 코드가 수정되면 하위 클래스의 코드도 수정되어야 하는 경우가 많습니다. (재정의 메소드)
* 확장이라는 목표를 두고 상속을 사용하면 괜찮으나, 해당 설계가 아닌 상황에서 상속을 사용할 시 문제를 발생시킬 수 있습니다.

### 구성(Composition)
* 하위 클래스가 상위 클래스를 상속받는 것이 아니라 단순히 하위 클래스가 상위 클래스를 private로 참조받아서 사용하는 것입니다.
* 그 후 필요한 메소드를 가져다 사용하는 것입니다. (포워딩 - wrapper class)

### 정리
* 상속은 상위 클래스와 하위 클래스가 순수한 is-a 관계일 때만 사용해야 합니다.
* is-a 관계라고해서 무조건 사용하는 것이 아니라 상위 클래스가 확장을 고려하여 설계되어 있어야 합니다.
* 상속의 취약점을 피하고 싶다면 Composition을 사용해야 합니다.
* 구성은 has-a 관계입니다.

## 접근제어자
### private
* 변수, 메소드는 해당 클래스에서만 접근이 가능합니다.

### default
* 접근제어자를 별도로 설정하지 않는다면 접근제어자가 없는 변수, 메소드는 default 접근제어자가 되어 해당 패키지 내에서만 접근이 가능합니다.

### protected
* 변수, 메소드는 동일 패키지내의 클래스 또는 해당 클래스를 상속받은 외부 패키지의 클래스에서 접근이 가능합니다.

### public
* 변수, 메소드는 어떤 클래스에서라도 접근이 가능합니다.

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
* Functional 인터페이스를 이용하면 람다를 이용하여 인스턴스를 생성할 수 있습니다.

## 익명 클래스와 람다
### 익명 클래스
* 익명 클래스는 말그대로 이름이 없는 클래스이며 new와 동시에 부모클래스를 상속받아 내부에서 오버라이딩해서 사용합니다.
* 매개변수로도 사용이 가능하며 상속은 받아야하지만 일회성으로 사용할 때 주로 사용합니다.
* 익명클래스의 this는 익명클래스 자신을 가리킵니다.

### 람다
* JDK 1.8 이후로는 함수 객체의 인스턴스를 람다식으로 만들어 사용할 수 있습니다.
* 람다는 인터페이스에 반드시 하나의 메서드만 가지고 있어야 합니다.
  * 인터페이스에 @FunctionalInterface 어노테이션을 붙이면 두 개 이상의 추상 메서드가 선언되었을 경우 컴파일 에러를 발생시킵니다.
* 람다에서의 this는 선언된 클래스를 가리킵니다.

## String과 new String
![1](https://raw.githubusercontent.com/smpark1020/backend-interview/master/Java/1.PNG)   

## Immutable Class(불변 클래스)
* 변경이 불가능한 클래스이며, 가변적이지 않는 클래스입니다.
* heap 영역에서 변경불가능한 것이지 재할당을 못하는 것은 아닙니다.
  * 재할당은 heap 영역의 객체가 바뀌는 것이지 heap 영역에 있는 값이 바뀌는 것이 아닙니다.
* String, Boolean, Integer, Float, Long 등등

### Immutable Class 만드는 방법
1. 클래스를 final로 선언
2. 멤버 변수를 private final로 선언
3. 접근 메서드를 구현하지 않는다. (Setter 메소드)
4. 객체를 생성하기 위한 생성자 또는 정적 팩토리 메소드를 추가
5. 참조에 의해 변경가능성이 있는 경우 방어적 복사를 이용하여 전달
```
public final class ImmutableClass {
    
    private final int age;
    private final String name;
    private final List<String> list;

    private ImmutableClass(int age, String name) {
        this.age = age;
        this.name = name;
        this.list = new ArrayList<>();
    }

    public static com.smpaaark.ImmutableClass of(int age, String name) {
        return new com.smpaaark.ImmutableClass(age, name);
    }

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public List<String> getList() {
        return Collections.unmodifiableList(list);
    }
    
}
```

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

## 싱글톤
### Singleton 패턴
* 인스턴스를 오직 한 개만 생성하는 패턴
* public static final 멤버로 딱 한번만 생성하는 방법
* private static final 멤버로 딱 한번만 생성한 후 정적 팩터리 메서드로 호출하는 방법

### 싱글톤 패턴을 사용하는 이유
* 메모리 낭비를 방지할 수 있습니다.
* 무조건 한번 생성으로 전역성을 띄기에 다른 객체와 공유가 용이합니다.

## 자동 형변환(묵시적 형변환)
* 작은 크기의 타입은 큰 크기의 타입으로 자동 형변환됩니다.

## 강제 형변환(명시적 형변환)
* 큰 크기의 타입을 작은 크기의 타입으로 변경할 경우 사용

## 기본 타입과 Wrapper 타입의 차이
### 기본 타입
* 기본 타입은 값만 가지고 있습니다.
* Stack에 저장됩니다.
* 0으로 초기화됩니다.
* 시간과 메모리 사용면에서 효율적입니다.

### Wrapper 타입
* 값에 더해 식별성을 갖습니다.
* heap에 저장됩니다.
* null로 초기화됩니다.
* NullPointerException 발생 위험이 있습니다.

## Local Variable, Instance Variable, Class Variable은 각각 JVM의 어디에 할당되는가?
* local variable은 Stack
* Instance variable은 Heap
* Class variable은 Method
* 변수가 참조 객체라면 메모리는 모두 Heap에 저장됨

## syncrhonized
* 멀티 스레드 환경에서 synchronzied 키워드를 사용하여 여러개의 스레드가 공유자원에 접근하는 것을 방지
* 메서드에 사용
* 블럭에 사용

## MyBatis에서 #{}과 ${}의 차이
* #{}은 PrparedStatement의 ? 에 세팅되어 입력됨
  * '파라미터' 형태
  * 사용자의 입력값을 파라미터로 전달할 경우 사용
* ${}은 입력 그대로 쿼리에 입력됨
  * 테이블 명이나 컬럼명을 동적으로 설정할 때 사용

## ConcurrentHashMap
* key, value 구조
* null을 허용하지 않음
* Entry 배열의 아이템 별로 동기화 처리됨
* 멀티 스레드 환경에 적합

## Atomic 변수
* CAS(Compare And Swap) 알고리즘으로 동기화 문제 해결
* 현재 스레드에 저장된 값과 메인 메모리에 저장된 값을 비교하여 일치하면 새로운 값으로 교체, 아니면 재시도

## HashMap 구조
* key와 value로 구성
* Hashing을 사용
* 해시 충돌 발생할 경우 연결 리스트를 사용