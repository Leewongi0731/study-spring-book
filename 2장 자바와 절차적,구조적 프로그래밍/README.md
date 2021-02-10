# 2장 자바와 절차적,구조적 프로그래밍

* 자바 실행 과정
  1. 사용자가 .java file에 소스코드 작성
  
  2. JDK(Java Development Kit)안에 javac.exe 컴파일러를 이용하여 .java file(Object file)을 생성
  
  3. JRE(Java Runtime Environment)안에 java.exe 프로그램 실행기를 이용하여 JVM위에서 실행
  
  4. JRE는 프로그램 안에 main() 메서드가 존재하는지 확인함, java.lang 패키지를 Static영역에 Load, Class를 Static영역에 Load
  
  5. Stack영역에 main()메소드를 적재, main()메소드 스택프레임 안에 파라미터 변수인 args스택생성
  
     
  
* 자바 T메모리 영역
  1. Static : 클래스 영역
  
  2. Stack : 메소드 영역
  
  3. Heap : 객체 영역
  
     
  
* 메소드 블랙박스화 : parameter, return에 의해서만 메소드 사이에 값이 전달될 뿐 서로 내부의 지역 변수를 볼 수 없다는 것을 의미

  

* 멀티 스레드(Multi Thread) : T메모리 영역의 Stack영역을 스레드 개수만큼 분할해서 쓰는 것
  - Stack영역만 나누는 것이기 떄문에 Static, Heap영역을 공유하기 떄문에 멀티 프로세스 대비 메모리를 적게 사용
  
  - 전역변수 문제점 : 스레드 A, 스레드 B가 전역변수를 각자 set하는 부분에서 값이 꼬일 수 있음
  
    
  
*  멀티 프로세스(Multi Process) : 다수의 T메모리 영역을 가지는 구조
  - 각 프로세스는 각자의 T메모리를 가지고 각 고유의 공간이므로 서로 참조할 수 없음
  - 멀티 스레드에 비해 높은 안정성을 보장, 하지만 많은 메모리를 사용