1. ## Framework 란?

  - 비기능적인 기능(인증, 로깅, Tx처리, 성능)을 제공할테니, 개발자는 biz 로직에 집중해라
   - 프레임워크의 구성요소
     - IoC(Inversion of Control) 제어의 역전
     - Design Pattern(GoF(gang of Four) pattern) 클래스 구조에 대한 Guide

2. ## Freamework와 Library의 차이점

  - 제어권을 누가 주도하느냐?

     - Library
     - 개발자
     - Framework
     - 프레임워크가 제공하는 컨테이너

   - 개발자가 작성한 클래스를 프레임워크의 컨테이너가 객체를 생성하고

     setter method를 호출한다.

3. ## IoC (Inversion of Control) : 제어권의 역전

  - IoC 컨테이너

  - IoC 구현 : DL (Dependency Lookup), DI(Dependency Injection)

4. ## DI

  - 용어 정의
     - Bean
       - 스프링이 IoC 방식으로 관리하는 객체
     - Spring Bean Container
       - BeanFactory
       - ApplcationContext
     - Configration MetaData
       - Config XML
       - Config 클래스
   - 의존성 주입
   - 의존하는 객체의 레퍼런스를 프레임워크가 생성해서 주입해 주겠다
   - 개발자는 의존하는 클래스의 정보를 XML or Java Config에 설정 해야함
   - 종류 및 주입 방식
     - Setter Injection
       - 기본생성자로 객체 생성 후 setter method의 인자로 의존하는 객체를 1개씩 주입 해주는 방식
     - Constructor Injection
       - Overloading된 생성자로 객체 생성 후 이 생성자의 인자로 의존하는 객체를 여러개씩 주입 해주는 방식
     - Method Injection
     - byte code injection
       - 프레임 워크가 대신 객체를 생성하면 특정 기능(singleton, tx처리)을 주입시켜줌

## DI 구현하는 방식

1. XML Based

   - bean 등록

   ```java
   <bean id="" class="" />
   ```

   - setter injection

   ```java
   <property value"" />
   <property ref="" />
   ```

   - constructor injection

   ```java
   <constructor-arg value="" />
   <constructor-arg ref="" />
   ```

2. Annotation Based (component-scan 설정을 위해서 xml 부분적으로 사용)

   - bean 등록 @Component

   - setter injection - 어노테이션을 변수, setter 메서드 위에 선언

   ​	@Value

   ​	@Autowired, @Qualifier

   - constructor injection - 어노테이션을 변수 옆에, 생성자 위에 선언

   ​	@Value, @Qualifier는 생성자의 변수 옆에

   ​	@Autowired는 생성자의 위에

3. Annotation Based (xml을 전혀 사용하지 않음)

   - Java 로 Configuration 클래스를 작성한다.

   ​	@Configuration

   - @Bean - 어노테이션으로 선언하지 않는 클래스를 Bean으로 등록
   - @ComponentScan - @Component 어노테이션으로 선언된 Bean을 찾을 때

## DI 구현하는 전략 3가지

- 전략 1

  - configuration(설정)을 xml에 한다.

    ```java
    <bean id="bean고유한이름"  class="package.class이름" />
        <!-- setter injection -->
        <bean id="hello" class="xx.Hello">  
             <!--setValue(Integer val) -->
             <property name="value"  value="100" />
             <!--setMyPrinter(Printer p) -->
             <property name="myPrinter" ref="strPrinter" />
        </bean>
       <bean id="strPrinter" class="xxx.StringPrinter" />
       
       <!-- Constructor injection -->
       public Hello(Integer val, String name, Printer pr) 
       <bean id="helloC" class="xxx.Hello">
            <constructor-arg name="val" value="100" />
            <constructor-arg name="name" value="스프링" />
            <constructor-arg name="pr" ref="strPrinter" />
       </bean>
    ```

  - 장점

    - 전체 의존관계 구조를 파악하기 쉬움

  - 단점

    - xml 파일 Sharing(공유)의 문제점

- 전략 2

  - annotation과 xml을 혼용해서 사용

    - @Component, @Controller, @Service, @Repository

      - <bean> 태그와 동일, bean으로 등록하겠다.

    - @Autowired(@Qualifier), @Resource / @Value

      - 의존관계가 있는 bean을 찾아서 자동으로 주입시켜 주는 기능

      - Autowired는 Type으로 찾음

      - Resource는 Name(id)으로 찾음

      - Resource는 변수, 메서드 위에 선언 가능

        ```java
        <context:component-scan base-package="" />
        ```

        - basepackage에서 지정한 패키지 아래의 모든 클래스에 선언된 @Component 등을 찾아주는 (Auto scanning) 기능

  - 장점

    - xml 설정이 좀 더 간단해져 관리가 용이
    - 개발모드에서 편리

  - 단점

    - Bean들간의 전체적인 의존관계를 파악하기 어려움

- 전략3 (ver3.0) Spring Boot

  - Java Config, xml을 사용하지 않음
  - @Configuration
    - Java Config 클래스
  - @Bean
    - <bean> 태그와 동일한 역할
  - @ComponentScan
    - <context:component-scan> 태그와 동일한 역할

5. ## Junit, Spring Test

  - Test case 작성 support

  - 자바기반 단위테스트를 지원하는 프레임워크

   - Test method

        - @Test
        - @Before
        - @After
        - @Ignore

   - Spring Test Framework를 사용

   - Junit 사용

     ```java
     BeanFactory factory = 
         new GenericXmlApplicationContext("config/beas.xml");
     Hello hello = factory.getBean("hello",Hello.class);
     ```

   - Spring Test 사용

     ```java
     @Runwith(SpringJunit4ClassRunner.class) 
     @ContextConfiguraion(locations="classpath:config/beans.xml")
     @Autowired
     ```

     - @Runwith
       - Junit 확장해서 테스트 케이스를 실행해주는 Runner를 지정할 때 SpringJunit4ClassRunner
     - @Autowired
       - 타입(type)으로 해당되는 Bean을 찾아서 주입해주는 어노테이션

---

Constructor Injection

Spring Test Framework

@Autowired, @Component

---

- alt + shift + l
  - 왼쪽에 타입 자동 생성

@Autowired

- 타입(type)으로 해당되는 Bean을 찾아서 주입해주는 어노테이션

@Qualifier 는 동일한 타입이 여러개가 있을 때 특정 Bean을 지정하는 어노테이션

- Bean의 id를 properties 에서 가져와서 사용하는 것이 지원되지 않는다.

```java
${myprinter}	// properties를 사용하는 것
myprinter=stringPrinter
```

@Resource

- Bean의 이름(id)으로 해당되는 Bean을 찾아서 주입해주는 어노테이션

- Bean의 id를 properties 에서 가져와서 사용하는 것이 지원된다.

## 현재까지 사용된 어노테이션 ---

@Test : test method

@Before : test method 전에 호출

@Runwith : Test Runner 를 확장할 때



@ContextConfiguration : Spring Bean xml 파일의 정보를 설정할 때

@Component, @Repository, @Service, @Controller

- Spring Bean 등록(생성)

@Value

- Spring Bean의 의존성 주입, 값을 주입

@Autowired / @Resource(javax.annotation)

- Spring Bean의 의존성 주입, Container가 의존하는 Bean을 찾아서 주입 해준다.

  reference를 주입

@Qualifier

- @Autowired와 같이 사용되며 , 특정 Bean을 지정한다.



- Spring Bean Configuration xml 을 사용하는 경우
  - GenericXmlApplicationContext - Spring Bean 컨테이너

- Spring Bean Configuration 클래스(no xml) 를 사용하는 경우
  - AnnotationConfigApplicationContext - Spring Bean 컨테이너