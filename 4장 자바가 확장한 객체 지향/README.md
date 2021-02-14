# 4장 자바가 확장한 객체 지향

- abstract class : 추상클래스로, 추상메소드를 생성 가능, 추상클래스는 인스턴스를 생성할 수 없다.

  - 추상메소드 : 메소드 선언은 있지만, 몸체는 없는 메소드
  - 추상메소드는 상속한 하위Class가 OverRiding하지 않으면 컴파일 시점에 에러가 발생함

- 생성자(객체 생성자 메소드)

  - 개발자가 아무런 생성자를 만들지 않으면, 자바에서는 기본생성자(파라미터 없는)를 만들어 지원함
  - 인자가 있는 생성자를 하나라도 만든다면, 기본 생성자를 만들어 주지 않는다.

- static block

  - 클래스가 로딩되고 클래스 변수가 준비된 후 자동으로 실행되는 블록, 주로 클래스 변수를 초기화시키는 코드를 둔다.

  - 한 클래스 안에 여러 개의 static 블록을 넣을 수 있다.

  - 예제

    ```java
    static class Student {
    
        static String name = "홍길동"; // 변수 선언에 값을 초기화시키는 문장을 포함할 수 있다.
        static {
            System.out.println("static...1");
            name = "임꺽정";
        }
    }
    
    public static void main(String[] args) {
        System.out.println("main 메서드 시작!");
    }
    
    실행 결과
    
    main 메서드 시작!
    ```

    * 객체멤버는 클래스가 static영역에 자리 잡은 후, 객체 생성자를 통해 힙에 생성됨

    * 클래스의 static불록이 실행되고 있을 때는 해당 클래스의 객체는 하나도 존재하지 않기 때문에 static블록에서는 객체 맴버 접근 할 수 없음

      ```java
      static class Student {
      
          static String name = "홍길동"; // 변수 선언에 값을 초기화시키는 문장을 포함할 수 있다.
          static {
              System.out.println("static...1");
              name = "임꺽정";
          }
          static {
              System.out.println("static...2");
              name = "유관순";
          }
      }
      
      public static void main(String[] args) {
          System.out.println("main 메서드 시작!");
          Student.name = "하하하";
          System.out.println(Student.name);
      }
      
      실행 결과
      
      main 메서드 시작!
      static...1
      static...2
      하하하
      ```

    * 클래스 정보는 해당 클래스가 코드에서 맨 처음 사용될 때 T메모리의 static영역에 로딩되며, 이때 **단 한번 ** 해당 클래스의 static불록이 실행됨

    * 클래스가 제일 처음 사용될 떄는 다음 세가지중 하나임

      1. 클래스의 정적 속성을 사용할 때
      2. 클래스의 정적 메서드를 사용할 때
      3. 클래스의 인스턴스를 최초로 만들 때

- instanceof  연산자

  - 만들어진 객체가 특정 클래스의 인스턴스인지 확인하는 연산자로 true, false를 반환함

    ex) 사람 이원기 = new 사람()

    if(이원기 instanceof 사람) --> true

- interface

  - interface는 정적 상수(static final)와 추상메소드(public abstract)만 가능함
  - 따로 명시하지 않아도, 정정 상수/추상메소드로 처리하여줌

- this 키워드

  - 지역 변수와 속성(객체 변수, 정적 변수)의 이름이 같은 경우 지역변수가 우선함
  - 객체 변수와 이름이 같은 지역 변수가 있는 경우 **객체 변수를 사용하려면 this를 접두사**로 사용함
  - 정적 변수와 이름이 같은 지역 변수가 있는 경우 **정적 변수를 사용하려면 클래스명을 접두사**로 사용함