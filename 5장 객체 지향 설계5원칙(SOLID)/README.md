# 5장 객체 지향 설계5원칙(SOLID)

* 객체 지향 설계 5원칙

  * 객체지향의 특성을 올바르게 사용하기 위해 설계된 원칙

  * 응집도를 높이고, 결합도를 낮추기 위한 고전 원칙을 객체지향 관점에서 재정립한 것

  * 응집도(cohesion) : 하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성으로, 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져 재사용이나 기능의 수정, 유지보수가 용이하다.

  * 결합도(coupling) : 상호 의존 정도로, 결합도가 낮다는 것은 모듈 간의 상호 의존성이 줄어들어 객체의 재사용이나 기능의 수정, 유지보수가 용이하는 것을 의미한다.

  * SoC(Separation Of Concerns) : 관심사 분리

    관심이 같은 것끼리는 하나의 객체안으로, 관심이 다른 것은 가능한 한 떨어져 서로 영향을 주지 않도록 분리해야 한다는 것으로, 하나의 속성/하나의 메소드/하나의 클래스/하나의 모듈/하나의 패키에는 하나의 관심사만 들어 있어야 한다는 것을 의미

  * SOLID 원칙을 작 적용하면 소스 파일의 개수가 더 많아 지는 경향이 있지만, 이는 논리를 더욱 잘 분할하고, 잘 표현하기에 이해하기 쉬우며 개발하기 쉽기 떄문에 유지/보수에 용의하다!

---



1. **Single Responsility Principle( SRP ) : 단일 책임 원칙**

   * "어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다."

   * [클래스, 속성, 메소드, 패키지, 모듈, 컴포넌트, 프레임워크 등]의 역할과 책임에 따라 분리하여 각각 하나의 역할과 책임을 갖게해야함

     ex) 메소드가 SRP를 지키지 않은 경우

     ```java
     public class 강아지 {
     	final static Boolean 숫컷 = true;
     	final static Boolean 암컷 = false;
     	Boolean 성별;
     
     	void 소변보다() {
     		if (this.성별 == 숫컷) {
     			// 한쪽 다리를 들고 소변을 본다.
     		} else {
     			// 뒤다리 두 개로 앉은 자세로 소변을 본다.
     		}
     	}
     }
     ```

     "소변보다()" 메소드가 수컷/암컷 강아지의 행위를 모두 구현하려고 했기에 단일 책임 원칙에 위배됨, 수컷/암컷 강이지 class를 따로 구분하고 해당 클레스의 각각의 "소변보다()" 메소드를 구현하는게 옳은 방법이다

2. **Open Cloed Principle( OCP ) : 개방 폐쇄 원칙**

   * "자신의 확장에는 열려 있고, 주변의 변화에 대해서는 닫혀 있어야 한다."

     ex 1) OCP의 예시 : JDBC

     ![JDBC](https://user-images.githubusercontent.com/48685242/108031871-e840a500-7074-11eb-9e6b-cc69c8bf5ce5.jpg)

     JDBC를 사용하는 클라이;언트는 데이터베이스가 오라클에서 MySQL이나 MS-SQL등 다른 데이터베이스로 변경되더라도, DB connection을 설정하는 부분 외에는 수정을 할 필요가 없다. 즉, 다른 DB로의 변경( 확장 )에 대해서는 열려있지만 코드 수정(변경)에 대해서는 닫혀있다는 좋은 예시다.

     ex 2) 편의점의 직원과 손님

     편의점에서 직원은 각 시간대에 따라 다른 직원으로 변경되지만, 손님입장에서 "구매하다"라는 행위에 영향은 없다. 직원입장에서도 다른 손님이 오더라도 "판매하다"라는 행위에 영향은 없다. 즉 손님과의 인테페이스가 바뀌지 않는 한 손님은 "구매하다"의 행위로 편의점을 사용할 수 있고, 직원은 "판매하다"로 편의점을 사용할 수 있다. 또한 직원의 구매 담당/보안 담당/청소 담당등 새로운 특징의 직원을 둘 수 있으므로, 확장에는 열려있는 좋은 예시다.

3. **Liskov Substitution Principle( LSP ) : 리스코프 치환 원칙**

   * "서브 타입은 언제나 자신의 기반타입(base type)으로 교체할 수 있어야한다."

   * Ch.3에서 정리한 "상속"에서의 두 조건을 만족하면 리스코프 치환 원칙을 잘 지키고있다 할 수 있음

     1. 하위 클래스 is a kind of 상위 클래스 : 하위 분류는 상위 분류의 한 종류다.
     2. 구현 클래스 is able to 인터페이스 : 구현 분류는 인터페이스할 수 있어야 한다.

     ex) 동물 뽀로로 = new 펭귄() 

     base type : 동물 / sub type : 펭귄

4. **Interface Segregation Principle( ISP ) : 인터페이스 분리 원칙**

   * "클라이언트는 자신이 사용하지 않는 메소드에 의존 관계를 맺으면 안 된다."

   ex) ISP가 지켜지지 않았을 경우 발생하는 문제

   ```java
   public interface MacPro {
     void display();
     void keyboard();
     void touch();
   }
   
   public class MacPro2016 implements MacPro {
     @Override
     public void display() {
       System.out.println("mac pro2016 display");
     }
   
     @Override
     public void keyboard() {
       System.out.println("mac pro2016 keyboard");
     }
   
     @Override
     public void touch() {
       System.out.println("mac pro2016 touch bar");
     }
   }
   
   public class MacPro2015 implements MacPro {
     @Override
     public void display() {
       System.out.println("mac pro2015 display");
     }
   
     @Override
     public void keyboard() {
       System.out.println("mac pro2015 keyboard");
     }
   
     @Override
     public void touch() {
       // ?? 
     }
   }
   ```

   ex) ISP를 지킨 코드

   ```java
   public interface MacPro {
     void display();
     void keyboard();
   }
   
   public interface MacProTouch {
     void touch();
   }
   
   public class MacPro2016 implements MacPro, MacProTouch {
     @Override
     public void display() {
       System.out.println("mac pro2016 display");
     }
   
     @Override
     public void keyboard() {
       System.out.println("mac pro2016 keyboard");
     }
   
     @Override
     public void touch() {
       System.out.println("mac pro2016 touch bar");
     }
   }
   
   public class MacPro2015 implements MacPro {
     @Override
     public void display() {
       System.out.println("mac pro2015 display");
     }
   
     @Override
     public void keyboard() {
       System.out.println("mac pro2015 keyboard");
     }
   }
   ```

5. **Dependency Inversion Principle( DIP ) : 의존 역전 원칙**

   * "고차원 모듈은 저차원 모듈에 의존하면 안된다. 두 모듈 모두 다른 추상화된 것에 의존해야 한다."

   * "추상화된 것은 구체적인 것에 의존하면 안된다. 구체적인 것이 추상화된 것에 의존해야 한다."

   * "자주 변경되는 구체화된 클래스에 의존하면 안된다."

   * 상위 클래스일수록, 인터페이스일수록, 추상 클래스일수록 변하지 않을 가능성이 높기에 하위 클래스나 구체화된 클래스가 아닌 상위 클래스일수록, 인터페이스일수록, 추상 클래스에 의존하라는 것

     ex 1) DIP적용 전/후

     | DIP 적용 전                                                  | DIP 적용 후                                                  |
     | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | ![DIP적용 전](https://user-images.githubusercontent.com/48685242/108036672-df070680-707b-11eb-98a7-13a298d95f9b.jpg) | ![DIP적용 후](https://user-images.githubusercontent.com/48685242/108036670-ddd5d980-707b-11eb-8485-e05e5eb32a67.jpg) |

     구체화된 클래스(스노우 타이어)는 시기가 지나면 자주 바뀌어야 한다. 하지만, DIP 적용 전과 같이 구체화된 클래스에 직접 의존하게 되면 타이어의 변경이 자동차에도 영향을 줄 수 있다. 그러므로 오른쪽과 같이 인터페이스에 의존하여 타이어를 사용하는 것이 좋다.