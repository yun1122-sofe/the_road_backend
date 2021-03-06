> 들어가며  
* 최범균 저자의 초보자 웹 개발자를 위한 스프링5 프로그래밍 입문 선택
* 스터디 참고용 도서 및 기본적인 내용을 학습하기 위해서  
  

> 스프링이란?  
* 의존 주입 지원(Dependency Injection : DI  
* AOP(Aspect-Oriented Programing) 지원  
* MVC 웹 프레임워크 제공  
* JDBC, JPA 연동, 선언적 트랜잭션 처리 등 DB 연동 지원  
  
> 스프링 기능  
* 스프링은 객체(=Bean)를 생성하고 초기화하는 기능을 제공함.  
  - 스프링이 관리하는 Bean 객체로 등록  
  - @Bean 들을 구분하기 위해서 메서드 이름을 사용한다.  
* 스프링 컨테이너 : `BeanFactory`, `ApplicationContext` 등.  
* 싱글톤 객체  
  - 내부적으로 해당 name의 빈이 없으면 새로 생성하고 있으면 읽어오게 되어 있다.  
> 스프링의 특징  
* 의존(Dependency Injection) : 객체간의 의존  
  - 한 클래스가 다른 클래스의 메서드를 실행할 때 이를 의존이라고 표현한다(ex. service -> dao의 메서드 호출)  
  > service클래스가 dao 클래스에 의존한다.  
  - 의존하는 대상이 있으면 구하는 방법이 필요한대 가장 쉬운 방법은 객체를 직접 생성하는 것
    - service 클래스에 dao객체를 생성해서 사용하는 모습.  
    - 이는 service 객체를 생성하면 동시에 dao객체도 생성하게 된다.  
    - 하지만 유지보수 관점에서, 문제점이 생길 수 있다.  
  - 의존을 구하는 방법 중 `Di`와 `서비스 로케이터` 방법도 있다. 
  > 의존  
  - 의존 객체를 전달받는 방식  
    - How? 생성자를 통해서.  
    - 생성자를 통해 의존 주입 받을 객체에 의존하고 있다.  
  - **왜, 직접 의존 객체를 생성하지 않고 굳이 생성자를 통해서 의존하는 객체를 주입하는 걸까?**  
    - 이유를 알려면 객체지향 설계에 대한 이해가 필요 -> 변경의 유연함  
    - 유지보수 측면에서, service와 dao의 의존관계에서 dao 의 변경이 발생한 경우 service에서도 수정을 해야한다.  
      - 직접 생성한 경우: 일일이 변경하게 되기 떄문에 유지보수가 힘들다.  
* 스프링은 DI르 지원하는 조립기이다.  
  - 책에서 Assembler(조립기) 해당하는 자바코드를 작성해봄.  
  - 조립기 -> 필요한 객체를 생성한 객체에 의존을 주입하는 기능.  
* DI 방식:  
  - 생성자 방식  
  - 세터 메서드 방식  
    - 파라미터 1개이다.  
    - 리턴 타입이 void다.  
    - [새터,게터 정보 득](http://bit.ly/22Rj2Ar)  
  - @Autowired  
    - 스프링의 자동 주입 기능, 해당 타입의 빈을 찾아서 필드에 할당함.  
    ```
    @Autowired
    private MemberDao memberDao;
    // MemberDao 타입에 해당하는 빈이 있는지 스프링 컨테이너 안에서 확인한다. 있으면 주입.
    ```
  - 자동주입  
    - @Autowired  
      - required라는 속성을 주어서 주입할 객체가 없을경우 exception을 발생시키는 것보다 null을 반환하도록 함.  
      - 이는 꼭 필요하지 않는 경우에 속성을 넣어주는 경우다. 이럴경우 자동주입을 수행하지 않는다.  
    - @Qualifier  
      - 한정자.  
    - Optional
      - 스프링5부터 자바 8의 Optional을 사용해도 됨.  
      - get()으로 값을 얻어올 수 있다.  
  - 필수 여부를 지정하는 세 가지 방법  
    - @Autowired(required=false)
    - Optional<>
    - @Nullable
    - @Autowired와 Nullable의 차이점은 null일 경우 @Autowired의 세터메서드는 호출하지 않고 @Nullabe달린 인자타입에 해당 빈이 없어도 이 메서드는 호출된다.  

> 설정정보  
* @Configuration : 해당 클래스를 스프링 설정 클래스로 지정한다.  
* AnnotationConfigApplicationContext 클래스  
  - 자바 클래스(@Configuration 애노테이션이 설정된)에서 정보를 읽어와 빈 객체를 생성하고 관리한다.(스프링의 핵심 기능)  
  - ApplicationContext 인터페이스에 정의되어 있는 것을 알맞게 구현한 클래스 중 하나.  
  - 구현된 AnnotationConfigApplicationContext 클래스는 설정 정보로부터 Bean이라고 불리는 객체를 생성하고 그 객체를 스프링 컨테이너 내부에 보관한다.  
  - 그리고 getBean()메서드를 실행하면 해당하는 빈 객체를 제공한다.  
  - 계층도에서 `BeanFactory` 인터페이스가 최상위에 위치하는데 객체 생성 및 검색에 대한 기능을 제공함.  
  - AnnotationConfigApplicationContext 클래스를 이용해서 스프링 컨테이너를 생성할 수 있다.  
    - getBean("Bean메서드이름", Bean 객체주소)  
    - getBean(Bean 객체주소) 으로도 사용할 수 있다.
      - 그렇지만, 같은 타입의 빈이 존재하지 않아야함.  
  - close() : 컨테이너 사용이 끝나면 컨테이너를 종료한다.  
    - close()메서드는 AbstractApplicationContext(AnnotationConfigApplicationContext, GenericXmlApplicationContext의  부모 클래스) 클래스에 정의되어 있다. 
  
* ApllicationContext 인터페이스  
  - 메세지, 프로필/환경 변수 등을 처리할 수 있는 기능을 추가로 정의  
  - 객체 생성, 초기화, 보관, 제거 등을 관리해서 `컨테이너` 라고 부른다.
  
* BeanFactory  
  - 생성된 객체를 검색하는데 필요한 getBean()메서드가 BeanFactory에 정의되어 있다.  
  - 싱글톤/프로토타입 빈인지 확인하는 기능도 제공  
  - getBean() 메서드를 실제 구현한 클래스는 AbstractApplicationContext.  
  
* 두 개의 설정  
  - 설정파일 2개 만들고 @Autowired를 사용하기
    - 설정 클래스가 2개 이상이어도 스프링 컨테이너를 생성하는 코드는 두 개의 설정파일 코드를 넣어주면 된다.  
    > ctxa = new AnnotationConfigApplicationContext(AppConf1.class, AppConf2.class);  
    - AnnotationConfigApplicationContext 생성자의 인자는 가변인자이기 때문에 콤마로 구분해서 전달하면 된다.  
  - @import 애노테이션 사용  
    - 함꼐 사용할 설저 클래스를 지정한다.
    - 때문에, 스프리 컨테이너를 생성할 때 AppConf2 설정 클래스를 지정할 필요가 없다.  
    - 배열을 이용해서 두 개 이상의 클래스도 지정할 수 있다.  

  - 다중 @Import : 설정 클래스가 바뀌어도 @import만 해준다면 스프링 컨테이너 생성하는 최상위 클래스가 바뀌는 일은 없다.  
    > @Import( {AppConf1.class, AppConf2.class} )  

> DB  
* 자바코드로 작성된 Map에 데이터를 넣고 실행하면, 메모리에 저장되는 것이기 때문에 프로그램 종료시 저장한 모든 회원 데이터가 사라진다.  
  - 프로그램을 종료해도 회원 데이터를 유지하려면 MySQL, Oracle DB에 저장해야 한다.  
  
***
> DI 정리  
* 스프링 컨테이너 안에 있는 모든 빈들은 고유하게 생성됨.  
  - 따라서, 타입이 일치하는 경우 @Qualifier을 통해 피해야 한다.  
  - 타입이 같고 이름이 같을 때 충돌(수동적 vs 자동적(컴포넌트스캔)
    - 컴포넌트 스캔의 경우  
      - 수동적이 승리  
  - 타입이 같은데 이름이 틀린경우(수동으로 Bean 작성, 컴포넌트 애노테이션으로 Bean등록)  
    - 스프링 컨테이너 안에는 두 개의 Bean이 존재하게 된다.  
    - 그러나 자동으로 주입의경우(@Autowired) 같은 타입으로 인한 Bean들의 선택충돌이 발생하지 않게 하기 위해 @Qualifier을 사용.  
* @Configuration  : 스프링 설정 클래스지정  ([참고](http://tech.javacafe.io/spring/2018/11/04/스프링-Configuration-어노테이션-예제/)  
  - 해다 클래스가 하나 이상의 @Bean 메소드를 제공하고 스프링 컨테이가 Bean정의를 생성하고 런타임시 그 Bean들이 요청들을 처리할 것을 선언.  
* @Bean : 해당 메서드가 생성한 객체를 스프링 빈이라고 설정.  
  - 메서드의 이름을 빈 객체의 이름으로 사용한다.  
* ComponentScan은 스캔해서 해당 클래스의 객체를 스프링 빈으로 등록한다.  
  - getBean을 사용해서 해당 클래스를 사용할 수 있다.
  - excludeFilters = @Filter(type = FilterTyp.REGEX, pattern="spring\\..*Dao"))
  - @Filter
    - type = FilterType.
      - ANNOTATION, classes={필터로 사용할 애노테이션 타입들} => 컴포넌트 스캔시 제외대상(1개 이상일 경우 배열로처리)  
      - ASSIGNABLE, classes= 제외할 타입목록(dao.class 등) => 특정 타입이나 그 하위 타입을 제외.  
      
     

***
