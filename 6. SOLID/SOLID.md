# SOLID 

<span style="font-familly:Papyrus;font-size:2em;">단일 책임 원칙 </span>

<br/>

개발자들은 지식과 경험으로 좋고 깔끔한 디자인의 어플리케이션을 설계한다. 그러나 시간이 지남에 따라 어플리케이션은 버그가 발생할 수 있다. 어플리케이션 디자인은 새로운 요청과 모든 변경 요청에 수정되어야 한다. 그리고 우리는 조금의 작업을 위해 전체 시스템의 지식과 어떻게 동작하는지 알기 위해 힘써야한다. 그러나 우리는 새로운 기능 요청을 탓할 수 없다. 그것은 소프트웨어 개발의 하나의 부분이며 멈추거나 거절할 수 없다. 그래서 왜이런 일이 발생하는걸까? 그것은 바로 어플리케이션의 설계이다.

<br/>
==========================================================
대부분 문제가 있는 소프트웨어는 아래의 과정을 따른다.
1. 과도한 책임을 클래스에게 주어 많은 스트레스를 준다. (클래스와 관련이 없는 매소드 등..)
2. 클래스들이 서로 의존하도록 강제한다. 클래스가 의존하는 경우(즉, 밀접하게 연관 된 경우) 하나의 변경 사항이 다른 클래스에 영향을 미침.
3. 시스템과 어플리케이션에 중첩된 코드 확산.
==========================================================

해결책
1. 정확한 아키텍쳐를 선택
2. 디자인원칙을 따른다.
3. 사양에 따라 소프트웨어를 구축하기 위한 올바른 디자인패턴을 선택한다.

SOLID 원칙
S : Single Responsibility Principle (SRP)
O : Open-Closed Principle (OCP)
L : Liskov Substitution Principle (LSP)
I : Interface Segregation Principle (ISP)
D : Dependency Inversion Principle (DIP)

***참고 : https://www.c-sharpcorner.com/UploadFile/damubetha/solid-principles-in-C-Sharp/

==========================================================

[<- Go to Home](../SUMMARY.md)