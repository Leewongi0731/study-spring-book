# 7장 스프링 삼각형과 설정 정보

* 스프링 삼각형
  - POJO( Plain Old Java Object )를 기반으로 IoC/DI, AOP, PSA라고하는 스프링의 3대 프로그래밍 모델



---



## IoC(Inversion of Control) / DI(Dependency Injection) : 제어의 역전 / 의존성 주입

| 목차                                                         |
| ------------------------------------------------------------ |
| [일반코드](#일반코드)                                        |
| [스프링 없이 의존성 주입하기. 생성자를 통한 의존성 주입](#스프링 없이 의존성 주입하기. 생성자를 통한 의존성 주입) |
| [스프링 없이 의존성 주입하기. 속성을 통한 의존성 주입](#스프링 없이 의존성 주입하기. 속성을 통한 의존성 주입) |
| [스프링을 통한 의존성 주입. XML파일 사용](#스프링을 통한 의존성 주입. XML파일 사용) |
| [스프링을 통한 의존성 주입. XML파일에서 속성 주입](#스프링을 통한 의존성 주입. XML파일에서 속성 주입) |
| [스프링을 통한 의존성 주입. @Autowired를 통한 속성 주입](#스프링을 통한 의존성 주입. @Autowired를 통한 속성 주입) |
| [스프링을 통한 의존성 주입. @Resource를 통한 속성 주입](#스프링을 통한 의존성 주입. @Resource를 통한 속성 주입) |

- 의존 관계란 변수에 값이 할당하는 모든 곳에서 생긴다. 다시말해, 대입연산자(=)에 의해 변수에 값이 할당되는 순간 의존이 생긴다는 것이다. 변수가 지역 변수이건, 할당되는 값이 리터럴이건 객체이건 의존은 발생한다. 이때 의존 대상이 내부에 있을 수도 있고, 외부에 있을 수도 있다.

- **DI(Dependency Injection)는 외부에 있는 의존 대상을 주입하는 것을 말한다.**

- 쉽게 말해 DI에서 말하는 의존 관계는 new로 표현된다고 생각하면 쉽다.

  ### 일반코드

  ```java
  # Tire.java
  interface Tire{
      String getBrand();
  }
  
  
  # KoreaTire.java
  public class KoreaTire implements Tire{
      public String getBrand(){ return "한국 타이어"; }
  }
  
  
  # AmericaTire.java
  public class AmericaTire implements Tire{
      public String getBrand(){ return "미국 타이어"; }
  }
  
  
  # Car.java
  public class Car{
      Tire tire;
      
      // Car.java에서 타이어를 생성(new)함으로, 의존관계가 일어나고 있음
      public Car(){ tire = new KoreaTire(); }
      
      public String getTireBrand(){ return "장착된 타이어 : " + tire.getBrand(); }
  }
  
  # Driver.java
  public class Driver{
      public static void main(String[] args){
          Car car = new Car();
          System.out.println( car.getTireBrand() );
      }
  }
  ```

  ---

  ### 스프링 없이 의존성 주입하기. 생성자를 통한 의존성 주입

  - **주입이란? : 예시처럼 자동차 내부에서 타이어를 생성하는 것이 아니라 생산된 타이어를 자동차에 장착하는 작업 **

  ```java
  # Car.java
  public class Car{
      Tire tire;
      
      // Car.java에서 타이어를 생성(new)하는 것이 아닌, 생성자 파라미터로 입력받아 사용. => 의존관계 X
      public Car(Tire tire){ this.tire = tire; }
      
      public String getTireBrand(){ return "장착된 타이어 : " + tire.getBrand(); }
  }
  
  # Driver.java
  public class Driver{
      public static void main(String[] args){
          // Car.java에서가 아닌, Car를 생성(new)하는 Class에서 Tire를 생성(new)하여 파라미터로 던짐
          Tire tire = new KoreaTire();
          Car car = new Car(tire);
          System.out.println( car.getTireBrand() );
      }
  }
  ```

  1번 예시의 방식과 같이 Car에서 Tire를 생성(new)하게 되면 해당 객체에 대해 정확히 알고 있어야 함(객체 생성을 해야하기 때문), 하지만 2번 예시와 같은 방식을 이용하여 "의존성을 주입"한다면,  Car은 그저 Tire 인터페이스를 구현한 어떤 객체가 들어오기만을 기다리고 사용하면 된다.

  추후에 다른 Tire Class들이 생기고, 상황에 따라 해당 Tire 생성자에 파라미터가 필요한 경우라도, Car.java의 소스코드는 변경하지 않아도 되기 때문에 때문에 확장성이 좋아진다. 

  ---

  ### 스프링 없이 의존성 주입하기. 속성을 통한 의존성 주입

  ```java
  # Car.java
  public class Car{
      Tire tire;
      
      public Tire getTire(){ return tire; }
      
      public void setTire(Tire tire){ this.tire = tire; }
      
      public String getTireBrand(){ return "장착된 타이어 : " + tire.getBrand(); }
  }
  
  # Driver.java
  public class Driver{
      public static void main(String[] args){
          // Car.java에서가 아닌, Car를 생성(new)하는 Class에서 Tire를 생성(new)하여 car의 setTire 함수를 통해 주입
          Tire tire = new KoreaTire();
          Car car = new Car();
          car.setTire(tire);        
          System.out.println( car.getTireBrand() );
      }
  }
  ```

  2번 예시에서는 생성된 Car 인스턴스의 tire를 다른 tire로 수정할 수가 없다. 하지만 3번 예시와 같은 방식을 이용하면 생성된 car 인스턴스에도 tire를 새롭게 적용 할 수 있다.

  ---

  ### 스프링을 통한 의존성 주입. XML파일 사용

  - Spring 도입 전 시퀀스 다이어그램

  ![Spring 도입 전 시퀀스 다이어그램](https://user-images.githubusercontent.com/48685242/108696967-98b31b00-7545-11eb-925e-5061cd599193.jpg)

  - Spring 도입 후 시퀀스 다이어그램

  ![Spring 도입 후 시퀀스 다이어그램](https://user-images.githubusercontent.com/48685242/108696963-9781ee00-7545-11eb-9831-3f7a9b7301f0.jpg)

  Spring을 통해 기존의 운전자가 타이/자동차를 직접 "생산"하던 시스템에서, 종합 쇼핑물을 통해 "구매"하는 형태로 바뀌었다. 

  ```java
  # Driver.java
     
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Driver{
      public static void main(String[] args){
          // "종합 쇼핑물"에 대한 정보
          ApplicationContext context = new ClassPathXmlApplicationContext("expert002/expert002.xml");
          
          // "종합 쇼핑물"에서 구매하는 코드
          Car car = context.getBean("car", Car.class);
          Tire tire = context.getBean("tire", Tire.class);
          
          car.setTire(tire);
          
          System.out.println( car.getTireBrand() );
      }
  }
  ```

  종합 쇼핑물에서 물건을 "구매"하기 위해서는 쇼핑물에 물건을 "등록"해야 한다. 물건에 대한 정보는 /src/main/java/expert002/expert002.xml에 존재하며 물건은 **<bean  id="tire" class="expert002.KoreaTire"></bean>**, **<bean  id="americaTire" class="expert002.AmericaTire"></bean>**와 같은 방법으로 등록 할 수 있다.  도식화한 표는 아래와 같다.

  ![Spring 의존성주입 시각화](https://user-images.githubusercontent.com/48685242/108714708-3b2ac880-755d-11eb-9ec1-66ed6dbd3715.png)

  이와 같이 XML에서 Bean을 id로 관리하기 때문에, 기존소스의 koreaTire 를 americaTire로 변경한다고 할 때, 코드의 변경/재컴파일/재배포할 필요없이 XML파일의 americaTire->tire, tire->koreaTire로 수정하면 된다.

  ---

  ### 스프링을 통한 의존성 주입. XML파일에서 속성 주입

  - Dirver.java의 car.setTire(tire)하던 부분을  XML의 property tag를 추가하여 주입할 수 있다.

  ```java
  # Driver.java
     
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Driver{
      public static void main(String[] args){
          // "종합 쇼핑물"에 대한 정보
          ApplicationContext context = new ClassPathXmlApplicationContext("expert003/expert003.xml");
          
          // "종합 쇼핑물"에서 구매하는 코드
          Car car = context.getBean("car", Car.class);
          
          // 타이어를 만들고, car 인스턴스에 주입하는 과정을 XML의 property tag를 이용하여 처리 할 수 있다.
  //        Tire tire = context.getBean("tire", Tire.class);
  //        car.setTire(tire);
          
          System.out.println( car.getTireBrand() );
      }
  }
  ```

  ![XML property Tag](https://user-images.githubusercontent.com/48685242/108723556-bee9b280-7567-11eb-8ba3-01d836f92b2e.png)

  1. 코드에서 Car 를 구매하는 부분
  2. Tire를 구매하는 부분
  3. id='koreaTire'와 ref='koreaTire'를 잇는 부분, 코리아 타이를 자동차의 타이어 속성에 결합하는 역할을 수행
  4. car의 tire속성을 설정하는 부분

  ---

  ### 스프링을 통한 의존성 주입. @Autowired를 통한 속성 주입

  - **@Autowired 어노테이션을 이용하면, 설정자 메소드를 이용하지 않고도 "종합쇼핑물"인 스프링 프레임워크가 설정 파일을 통해 설정자 메서드 대신 속성을 주입해준다!**

  - @Autowired 어노테이션을 이용하면, car의 property tag을 이용하지 않고도 의존성 주입이 가능하다.

    ```java
    # Driver.java
    import org.springframework.beans.factory.annotation.Autowired;
    
    # Car.java
    public class Car{
        @Autowired
        Tire kTire;
        
        public String getTireBrand(){ return "장착된 타이어 : " + tire.getBrand(); }
    }
    ```

    ```xml
    <bean id="kTire" class="expert004.KoreaTire"></bean>
    <bean id="americaTire" class="expert004.AmericaTire"></bean>
    <bean id="car" class="expert004.Car"></bean>
    ```

  - @Autowired 어노테이션은 어떠한 순서로 의존성을 주입시켜 주는 것인가?

    ![Autowired 동작순서](https://user-images.githubusercontent.com/48685242/108731373-d75dcb00-756f-11eb-9746-b34ffe4940a7.png)

    1. 해당 Type이랑 겹치는 Bean이 존재하는가 확인.
    2. 해당 Type의 Bean이 오직 한개인 경우는 id 매칭을 시도하지 않고 바로 할당
    3. 해당 Type의 Bean이 하나가 아니라 여러개라면, id 매칭을 시도하고 알맞는 Bean을 할당

  - @Autowired 어노테이션 적용가능 경우 가능/불가능 경우 예시

    ```xml
    # Car.java
    @Autowired
    Tire tire;
    
    # expert.xml
    <bean class="expert004.KoreaTire"></bean>
    <bean id="americaTire" class="expert004.AmericaTire"></bean>
    
    --> Tire Type이 2개이고, "tire"의 id를 가지는 Bean이 없으므로 No unique bean Error
    //////////////////////////////////////////////////////////////////////////////
    # Car.java
    @Autowired
    Tire ktire;
    
    # expert.xml
    <bean id="kTire" class="expert004.KoreaTire"></bean>
    <bean id="americaTire" class="expert004.AmericaTire"></bean>
    
    --> Tire Type이 2개이고, "tire"의 id를 가지는 Bean이 있으므로 KoreaTire을 할당
    //////////////////////////////////////////////////////////////////////////////
    # Car.java
    @Autowired
    Tire tire;
    
    # expert.xml
    <bean id="koreaTire" class="expert004.KoreaTire"></bean>
    
    --> Tire Type이 1개 이미로, id 매칭을 시도하지 않고 KoreaTire을 할당
    ```

  ---

  ### 스프링을 통한 의존성 주입. @Resource를 통한 속성 주입

  - @Autowired 어노테이션은 **Spring 어노테이션**, @Resource어노테이션은 **Java 표준 어노테이션**
  - Spring을 사용하지 않을 것이라면, @Autowired 어노테이션은 사용할 수 없고 @Resource어노테이션을 사용해야 함
  - @Autowired 어노테이션은 Type, Id중 Type에 우선순위를 부여하여 할당하지만, @Resource어노테이션은  Type, Id중 Id에 우선순위를 부여하여 할당한다는 차이점이 있음
  - 인스턴스 명칭을 id mapping하는 것이 아니라, 다른 이름으로 id에 mapping하기 위해서는 @Autowired 어노테이션은 @Qualifier("pickBeanId")으로 지정가능하고, @Resource어노테이션은 @Resource(name="pickBeanId")로 가능하다.

