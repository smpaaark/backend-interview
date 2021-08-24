## Spring
* [트랜잭션 전파 속성](https://github.com/smpark1020/backend-interview/tree/master/Spring#%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%A0%84%ED%8C%8C-%EC%86%8D%EC%84%B1)
* [트랜잭션 격리 수준]()
* [AOP]()
* [Filter, Interceptor, AOP 차이]()
* [DispatcherServlet]()
* [의존성 주입 방법]()
* [Bean Scope]()
* [Spring에서 싱글톤이 좋은 이유]()
* [외부 API 테스트]()

[뒤로](https://github.com/smpark1020/backend-interview#back-end-interview)

## 트랜잭션 전파 속성
진행되고 있는 트랜잭션에서 다른 트랜잭션이 호출될 때 어떻게 처리할지 정하는 것입니다.

### 전파 설정 옵션
\@Transactional의 옵션 propagation을 통해 설정할 수 있습니다.

* REQUIRED(기본값)
  * 부모 트랜잭션이 존재한다면 부모 트랜잭션으로 합류합니다.
  * 부모 트랜잭션이 없다면 새로운 트랜잭션을 생성합니다.
  * 중간에 롤백이 발생한다면 모두 하나의 트랜잭션이기 때문에 진행사항이 모두 롤백됩니다.
* REQUIRES_NEW
  * 무조건 새로운 트랜잭션을 생성합니다.
  * 각각의 트랜잭션이 롤백되더라도 서로 영향을 주지 않습니다.
* MANDATORY
  * 부모 트랜잭션에 합류합니다.
  * 만약 부모 트랜잭션이 없다면 예외를 발생시킵니다.
* NESTED
  * 부모 트랜잭션이 존재한다면 중첩 트랜잭션을 생성합니다.
  * 중첩된 트랜잭션 내부에서 롤백 발생시 해당 중첩 트랜잭션의 시작 지점까지만 롤백됩니다.
  * 중첩 트랜잭션은 부모 트랜잭션이 커밋될 때 같이 커밋됩니다.
  * 부모 트랜잭션이 존재하지 않는다면 새로운 트랜잭션을 생성합니다.
  * 중첩 트랜잭션이 끝난 후 부모 트랜잭션에서 롤백이 발생하면 모든 트랜잭션을 롤백합니다.
* NEVER
  * 트랜잭션을 생성하지 않습니다.
  * 부모 트랜잭션이 존재한다면 예외를 발생시킵니다.
  * 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않고 기능을 진행합니다.
* SUPPORTS
  * 부모 트랜잭션이 있다면 합류합니다.
  * 진행중인 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않습니다.
* NOT_SUPPORTED
  * 부모 트랜잭션이 있다면 보류시킵니다.
  * 진행중인 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않습니다.

### 참조
* [[Spring] 트랜잭션의 전파 설정별 동작](https://deveric.tistory.com/86)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## 트랜잭션 격리 수준
* 트랜잭션을 사용할 때, 전파 속성과 함께 따라오는 것이 격리 수준입니다.
* 트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준을 말합니다.
* 격리 수준이 높아질수록 동시성(Concurrency)은 높아지고 속도는 느려집니다.
* 이 둘의 균형을 잘 맞추는 것이 중요합니다.

### READ_UNCOMMITED (커밋되지 않는 읽기, level 0)
* 트랜잭션 처리중 or 아직 commit되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용합니다.
* Dirty Read 발생
  * 다른 트랜잭션에서 처리하는 작업이 완료되지 않았는데도 다른 트랜잭션에서 읽을 수 있는 현상
  * READ_UNCOMMITED 격리 수준에서만 발생

### READ_COMMITED (커밋된 읽기, level 1)
* Dirty Read 방지: 트랜잭션이 commit 되어 확정된 데이터만을 읽도록 허용
* A라는 데이터가 B로 변경되는 동안 다른 사용자는 해당 데이터에 접근할 수 없습니다.

### REPEATABLE_READ (반복 가능한 읽기, level 2)
* 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸립니다.
  * 따라서 다른 사용자는 해당 영역에 대한 데이터 수정이 불가능합니다.
* 선형 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 후행 트랜잭션이 갱신하거나 삭제하는 것을 불허함으로써 같은 데이터를 두 번 쿼리했을 때 일관성 있는 결과를 리턴합니다.
* Phantom READ 발생
  * 트랜잭션이 데이터를 두 번 읽는다고 가정할 때 where 절의 조건에 맞는 레코드가 추가로 생성될 때 새로운 "유령(phantom)"행이 나오지만 phantom read를 지원하지 않으면 새로 생긴 행을 볼 수 없습니다.

### SERIALIZABLE (직렬화 가능, level 3)
* 완벽한 읽기 일관성 모드 제공(가장 엄격함)
* 성능 측면에서 동시 처리성능이 가장 낮습니다.
* 거의 사용되지 않습니다.
* 데이터의 일관성 및 동시성을 위해 MVCC(Multi Version Concurrency Control)를 사용하지 않습니다.
* 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 shared lock이 걸리므로 다른 사용자는 그 영역에 해당하는 데이터에 대한 수정 및 입력이 불가능합니다.
* MVCC
  * 다중 사용자 DB 성능을 위한 기술
  * 데이터 조회시 LOCK을 사용하지 않고, 데이터의 버전을 관리해 데이터의 일관성 및 동시성을 높이는 기술

### 각 DB별 isolation
* MYSQL: REPEATABLE_READ
* ORACLE: READ_COMMITED
* H2: READ_COMMITED

### 참조
* [[Spring] @Transactional - 2 isolation (격리수준)](https://n1tjrgns.tistory.com/267)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## AOP
* Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불립니다.
* 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것입니다.
* 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말합니다.
* 핵심적인 관점은 우리가 적용하고자 하는 핵심 비즈니스 로직이 됩니다.
* 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있습니다.
* AOP에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미입니다.
* 이때 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 반결할 수 있는데 이것을 흩어진 관심사(Crosscutting Concerns)라 부릅니다.
* 흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지입니다.

### 사용 예시
\@Transactional, 성능 측정, 로깅 등등

### AOP 주요 개념
* Aspect
  * 흩어진 관심사를 모듈화 한 것
  * 주로 부가기능을 모듈화함
* Target
  * Aspect를 적용하는 곳(클래스, 메소드 ...)
* Advice
  * 실질적으로 어떤 일을 해야할 지에 대한 것
  * 실질적인 부가기능을 담은 구현체
* JointPoint
  * Advice가 적용될 위치, 끼어들 수 있는 지점
  * 메소드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용 가능
* PointCut
  * JointPoint의 상세한 스펙을 정의한 것
  * 'A라는 메소드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있음

### 스프링 AOP 특징
* 프록시 패턴 기반의 AOP 구현체
  * 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위해서임
* 스프링 빈에만 AOP를 적용 가능
* 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가, ...)에 대한 해결책을 지원하는 것이 목적

### 스프링 AOP: \@AOP
* 스프링 \@AOP를 사용하기 위해서는 의존성을 추가해야 합니다.
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
* 다음에는 \@Aspect 어노테이션을 붙여 이 클래스가 Aspect를 나타내는 클래스라는 것을 명시하고 \@Component를 붙여 스프링 빈을 등록합니다.
* \@Around 어노테이션은 타겟 메소드를 감싸서 특정 Advice를 실행한다는 의미입니다.
  * execution은 이 Aspect를 어느 패키지의 어느 객체의 어느 메소드에 적용하겠다라는 설정입니다.
  ```
  @Component
  @Aspect
  public class PerfAspect {

    @Around("execution(* com.saelobi..*.EventService.*(..))")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable{
      long begin = System.currentTimeMillis();
      Object retVal = pjp.proceed(); // 메서드 호출 자체를 감쌈
      System.out.println(System.currentTimeMillis() - begin);
    
      return retVal;
    }

  }
  ```
* 또한 경로지정 방식말고 특정 어노테이션이 붙은 포인트에 해당 Aspect를 실행할 수 있는 기능도 제공합니다.
```
@Component
@Aspect
public class PerfAspect {

    @Around("@annotation(PerLogging)")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable{
        long begin = System.currentTimeMillis();
        Object retVal = pjp.proceed(); // 메서드 호출 자체를 감쌈
        System.out.println(System.currentTimeMillis() - begin);
        return retVal;
    }

}
```
* 마찬가지로 스프링 빈의 모든 메소드에 적용할 수 있는 기능도 제공합니다.
```
@Component
@Aspect
public class PerfAspect {

    @Around("bean(simpleEventService)")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable{
        long begin = System.currentTimeMillis();
        Object retVal = pjp.proceed(); // 메서드 호출 자체를 감쌈
        System.out.println(System.currentTimeMillis() - begin);
        return retVal;
    }

}
```

* 이 밖에도 \@Around 외에 타겟 메소드의 Aspect 실행 시점을 지정할 수 있는 어노테이션이 있습니다.
  * \@Before(이전): 어드바이스 타겟  메소드가 호출되기 전에 어드바이스 기능을 수행
  * \@After(이후): 타겟 메소드의 결과에 관계없이(즉 성공, 예외 관계없이) 타겟 메소드가 완료되면 어드바이스 기능을 수행
  * \@AfterReturning(정상적 반환 이후): 타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
  * \@AfterThrowing(예외 발생 이후): 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
  * \@Around(메소드 실행 전후): 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출 전과 후에 어드바이스 기능을 수행

### 참조
* [[Spring] 스프링 AOP (Spring AOP) 총정리 : 개념, 프록시 기반 AOP, @AOP](https://engkimbs.tistory.com/746)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## Filter, Interceptor, AOP 차이
스프링에서 사용되는 Filter, Interceptor, AOP 세 가지 기능은 모두 무슨 행동을 하기전에 먼저 실행하거나, 실행한 후에 추가적인 행동을 할 때 사용되는 기능들입니다.

### Filter, Interceptor, AOP의 흐름
* Interceptor와 Filter는 Servlet 단위에서 실행됩니다.
  * 반면 AOP는 메소드 앞에 Proxy 패턴의 형태로 실행됩니다.
* 실행 순서는 Filter가 가장 밖에 있고 그 안에 Interceptor, 그 안에 AOP가 있는 형태입니다.
* 따라서 요청이 들어오면 Filter -> Interceptor -> AOP -> Interceptor -> Filter 순으로 거치게 됩니다.

1. 서버를 실행시켜 서블릿이 올라오는 동안에 init이 실행되고, 그 후에 doFilter가 실행됩니다.
2. 컨트롤러에 들어가기 전에 preHandler가 실행됩니다.
3. 컨트롤러에서 나와 postHandler, after Completion, doFilter 순으로 진행이 됩니다.
4. 서블릿 종료 시 destroy가 실행됩니다.

### Filter, Interceptor, AOP의 개념
1. Filter(필터)
  * 말그대로 요청과 응답을 거른 뒤 정제하는 역할을 합니다.
  * 서블릿 필터는 DispatcherServlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청 내용을 변경하거나, 여러가지 체크를 할 수 있습니다.
  * 또한 자원의 처리가 끝난 후 응답내용에 대해서도 변경하는 처리를 할 수 있습니다.
  * 보통 web.xml에 등록하고 일반적으로 인코딩 변환 처리, XSS방어 등의 요청에 대한 처리로 사용됩니다.
  * 필터의 실행 메소드
    * init() - 필터 인스턴스 초기화
    * doFilter() - 전/후 처리
    * destroy() - 필터 인스턴스 종료

2. Interceptor(인터셉터)
  * 요청에 대한 작업 전/후로 가로챈다고 보면 됩니다.
  * 필터는 스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 작동합니다.
  * 하지만 인터셉터는 스프링의 DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어들기 때문에 스프링 컨텍스트(Context, 영역) 내부에서 Controller(Handler)에 관한 요청과 응답에 대해 처리합니다.
  * 스프링의 모든 빈 객체에 접근할 수 있습니다.
  * 인터셉터는 여러 개를 사용할 수 있고 로그인 체크, 권한 체크, 프로그램 실행시간 계산작업 로그확인 등의 업무처리에 사용됩니다.
  * 인터셉터의 실행 메소드
    * preHandler() - 컨트롤러 메소드가 실행되기 전
    * postHandler() - 컨트롤러 메소드 실행 직후 view 페이지 렌더링 되기 전
    * afterCompletion() - view페이지가 렌더링 되고 난 후

3. AOP
* OOP를 보완하기 위해 나온 개념
* 객체 지향의 프로그래밍을 했을 때 중복을 줄일 수 없는 부분을 줄이기 위해 관점에서 바라보고 처리합니다.
* 주로 '로깅', '트랜잭션', '에러 처리' 등 비즈니스단의 메소드에서 조금 더 세밀하게 조정하고 싶을 때 사용합니다.
* Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정이 가능합니다.
* Interceptor와 Filter는 주소로 대상을 구분해서 걸러내야하는 반면, AOP는 주소, 파라미터, 어노테이션 등 다양한 방법으로 대상을 지정할 수 있습니다.
* AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이입니다.
* Advice의 경우 JointPoint나 ProccedingJointPoint 등을 활용해서 호출합니다.
* 반면 HandlerInterceptor는 Filter와 유사하게 HttpServletRequest, HttpServletResponse를 파라미터로 사용합니다.
* AOP의 포인트 컷
  * \@Before: 대상 메소드의 수행 전
  * \@After: 대상 메소드의 수행 후
  * \@After-returning: 대상 메소드의 정상적인 수행 후
  * \@After-throwing: 예외 발생 후
  * \@Around: 대상 메소드의 수행 전/후
  
### 참조
* [[Spring] Filter, Interceptor, AOP 차이 및 정리](https://goddaehee.tistory.com/154)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## DispatcherServlet
### DispatcherSevlet의 개념
* dispatch는 보내다라는 뜻을 가지고 있습니다.
* 가장 앞단에서 HTTP 프로토콜로 들어오는 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)라고 정의할 수 있습니다.
* 클라이언트로부터 어떠한 요청이 오면 Tomcat(톰캣)과 같은 서블릿 컨테이너가 요청을 받게 됩니다.
* 그리고 이 모든 요청을 먼저 프론트 컨트롤러인 디스패처 서블릿이 받게 됩니다.
* 그러면 디스패치 서블릿은 공통적인 작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 세부 컨트롤러를 getBean()으로 가져오고, 정해진 메소드를 실행시켜 작업을 위임합니다.
* 예를 들어 예외가 발생하였을 때 일관된 방식으로 처리하는 것도 프론트 컨트롤러인 디스패처 서블릿이 담당하고 있습니다.
* Front Controller
  * 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러로써, MVC 구조에서 함께 사용되는 디자인 패턴입니다.

### DispatcherServlet의 장점
* Spring MVC는 DispatcherServlet이 등장함에 따라 web.xml의 역할을 상당히 축소시켜주었습니다.
* 기존에는 모든 서블릿에 대해 URL 매핑을 활용하기 위해서 web.xml에 모두 등록해주어야 했지만, DispatcherServlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고 공통 작업을 처리하면서 상당히 편리하게 이용할 수 있게 되었습니다.

### DispatcherSevlet 흐름
1. DispatcherServlet은 web.xml에 정의된 URL 패턴에 맞는 요청을 받고 URL 컨트롤러의 맵핑 작업을 HandlerMapping에 요청
2. HandlerMapping은 URL을 기준으로 어떤 컨트롤러를 사용할지 결정, 결과는 HandlerExcution Chain 객체에 담아서 리턴하는데 요청에 해당하는 Interceptor가 있을 경우 함께 줌
3. HandlerAdapter는 컨트롤러의 메소드를 호출하는 역할을 하는데 실행될 Interceptor가 있을 때는 Interceptor의 preHandle() 메소드를 실행한 다음 컨트롤러의 메소드를 호출하여 요청  처리
4. 컨트롤러는 요청을 처리한 뒤 처리한 결과 및 ModelAndView를 DispatcherServlet에 전달
5. ViewResolver는 컨트롤러가 처리한 결과를 보여줄 뷰를 결정. 컨트롤러에서 전달받은 View 이름의 앞뒤로 prefix, suffix 프로퍼티를 추가한 값이 실제 사용할 뷰 파일 경로가 됨. ViewResolver는 맵핑되는 View 객체를 DispatcherServlet에 전달
6. DispatcherServlet은 ViewResolver에 전달받은 View Model을 넘겨서 클라이언트에게 보여줄 화면을 생성

### 참조
* [[Spring]Dispatcher-Servlet(디스패처 서블릿)이란?](https://mangkyu.tistory.com/18)
* [[Spring] Spring 웹 요청 흐름(DispatcherServlet)](https://rutgo.tistory.com/231)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## 의존성 주입 방법
### 수정자 주입(Setter)
* 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 거의 없습니다.
* 하지만 Setter는 언제든 변경되게 할 위험이 있습니다. (자칫하면 치명적인 버그로 이어질 수 있습니다.)
* 수정자 주입을 사용하면 setter 메소드를 public으로 열어두어야 합니다.
* 즉 언제 어디서든 변경이 가능하다는 뜻입니다.

### 필드 주입
* 코드가 간결하다는 장점이 있지만 외부에서 변경이 불가능해서 테스트 하기 힘들다는 치명적인 단점이 있습니다.
* DI 프레임워크가 없으면 아무것도 할 수 없습니다.

### 생성자 주입
* Setter 메소드에 \@Autowired 어노테이션을 붙입니다.
* 

### 생성자, 필드, Setter 주입 중 어떤 방법이 가장 좋은지
* 생성자 호출시점에 딱 1번만 호출되는 것이 보장됩니다.
* 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없습니다.
* 따라서 불변하게 설계할 수 있습니다.
* 생성자 주입을 사용하면 의존성을 주입을 누락하는 것을 방지할 수 있습니다.
  * IDE에서 컴파일 오류로 알려줍니다.
* 생성자 주입을 활용할 경우 값의 불변을 보장할 수 있으므로 final을 활용할 수 있습니다.
* 생성자를 통해 의존성을 설정하고 변경할 일이 없으므로 final로 안전하게 불변을 보장할 수 있습니다.
* 결국 생성자 주입을 이용하면 원하는 구현체를 주입할 수 있으며, 순수 자바 코드로 테스트를 실행할 수 있습니다.

### 정리
* 생성자 주입 방식을 선택하는 이유는 여러가지가 있지만, 프레임워크에 의존하지 않고 순수한 자바 언어의 특징을 잘 살리는 방법입니다.

### 참조
* [[Spring] 생성자 주입이 좋은 이유와 다양한 DI 방법](https://woodcock.tistory.com/8)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## Bean Scope
* Spring의 Bean은 별다른 설정이 없으면 Singleton Scope으로 생성됩니다.
* 특정 타입의 Bean을 하나만 만들어 두고 공유해서 사용하기 위해서인데 이러한 까닭에 Bean에 상태를 저장하는 코드를 작성하는 것은 동시성 문제를 유발하여 위험한 상태를 초래할 수 있습니다.
* 하지만 요구사항과 구현 기능 등의 필요에 따라서 비싱글톤이 필요한 경우도 많습니다.
* 그리고 이를 명시적으로 구분하기 위해서 scope이라는 키워드를 제공합니다.

### Scope의 종류
* 싱글톤
  * Spring 프레임워크에서 기본이 되는 스코프
  * 스프링 컨테이너 시작과 종료까지 1개의 객체로 유지됩
* 프로토타입
  * 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 스코프
  * 요청이 오면 항상 새로운 인스턴스를 생성하여 반환하고 이후에 관리하지 않음
  * 프로토타입을 받은 클라이언트가 객체를 관리해야 함
* 웹
  * request: 각각의 요청이 들어오고 나갈때까지 유지되는 스코프
  * session: 세션이 생성되고 종료될 때까지 유지되는 스코프
  * application: 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프

### 참조
* [[Spring] Bean Scope(빈 스코프)의 종류](https://mangkyu.tistory.com/117)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## Spring에서 싱글톤이 좋은 이유
### 웹 애플리케이션에서 싱글톤이 좋은 이유
* 웹 애플리케이션의 특징 중 하나는 여러 종류의 요청을 동시에 처리해야 한다는 것입니다.
* 만약 싱글톤 객체를 쓰지 않는다면 요청마다 요청에 필요한 객체들을 새로 생성해야 할 것이고 이 객체들은 요청이 끝난 후 gc의 대상이 될 것입니다.
* 요청이 많아지면 빈번한 gc로 인해 시스템에 악영향을 끼칠 수 있습니다.

### 싱글톤 패턴 주의점
싱글톤 패턴은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체가 상태를 유지하면 안됩니다.
* 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안됩니다.
* 읽기만 가능해야 합니다.
* 클래스 변수가 아닌 지역변수, ThreadLocal 등을 사용해야 합니다.

### 싱글톤 패턴 단점
* 클라이언트가 구체 클래스에 의존합니다.
* 여러 테스트 세트 내에서 하나의 인스턴스가 그 상태를 유지하기 때문에 테스트가 어렵습니다.
* private 생성자 때문에 자식 클래스를 만들기 어렵습니다.

### 싱글톤 컨테이너
* 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리합니다.
* 스프링 컨테이너 덕분에 싱글톤 패턴의 단점들을 해결하면서 객체를 싱글톤으로 유지할 수 있습니다.

### \@Configuration, \@Bean
* 빈은 스프링이 cglib 라이브러리로 만드는 프록시 클래스입니다.
* 프록시 클래스 내에서 \@Bean이 붙은 메소드 마다 이미 빈이 존재하면 존재하는 빈을 반환하고, 빈이 없다면 생성해서 빈으로 등록하고 반환하는 코드가 동적으로 만들어 집니다.

### 참조
* [싱글톤 컨테이너](https://5min.medium.com/%EC%8B%B1%EA%B8%80%ED%86%A4-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-d7c469d1fa4f)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## 외부 API 테스트
* 보통 외부 자원을 사용하는 메소드를 테스트 코드로 작성할 때 외부 자원 사용 코드를 Mocking 합니다.
* MockRestServiceServer라는 임시 서버를 사용하여 어떤 요청이 올 때 어떤 데이터를 반환할지를 지정합니다.
* 테스트를 실행하고 외부 API 연동 기능이 실행되면 MockRestServiceServer가 Mocking 하여 지정한 결과를 반환해줍니다.
* 이 때 RestTemplate은 RestTemplateBuilder를 주입받아야 합니다.

### 참조
* [Spring Boot에서 외부 API 테스트하기](https://jojoldu.tistory.com/341)

[맨위로](https://github.com/smpark1020/backend-interview/tree/master/Spring#spring)

## 스프링 빈의 동시성 문제