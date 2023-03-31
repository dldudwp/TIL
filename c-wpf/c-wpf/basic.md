# Basic





**메모리 영역**

\| 코드 영역 |  : 명령이 저장

\| 데이터영역 | : 전역 변수, 전역 객체, static이 지정된 변수 및 객체가 저장

\| 스택 영역 | : 인수, 지역변수 및 객체가 저장

\| 힙 영역 | : new 연산자를 사용하여 생성된 변수 및 객체



**객체**

1. 전역 객체 : 함수 외부에서 생성
   * 프로그램이 시작 시 생성되어 프로그램이 실행중에 메모리 상에 존재 프로그램이 종료할 때 삭제
2. 지역 객체 : 함수 내부에서 static을 붙이지 않고 생성된 것
   * 선언하는 함수 내에서 작업이 진행된 시점에 생성되어 함수종료 시 삭제
3. 정적 객체 : 함수 내부에서 static을 붙여 생선된 것&#x20;
   * 선언하는 함수에서 작업이 진행된 시점에 한번 생성되어 프로그램이 실행 중 항상 메모리에 존재
4. 동적 객체 : new 연산자로 생성된 것
   * new 연산자로 생성되어 delete 연산자로 삭제 되기 전까지 메모리에 존재



**상속**

* 성 및 소멸자 호출 순서
  * 기본클래스 생성자 -> 파생클랙스 생성자 -> 파생클래스 소멸자 -> 기본클래스  소멸자 순이다.
*

**컴포지션**

* 다른클래스의 객체를 멤버로 가진 것
* 상속은 is-a 관계, 컴포지션은 has-a 관계
* 생성 및 소멸자 호출 순서
  * 멤버객체 생성자 -> 소유자 생성자 -> 소유자 소멸자 -> 멤버객체 소멸자 순이다.

멤버객체가 인수를 가진 생성자를 포함하는 경우

멤버객체를 가진 클래스 생성자에서 멤버 초기화를 설정

ex)  employee 가 handphone 객체를 가짐

```
Employee:Employee(int n , char* b, int i )
{
    strcpy(hp.phone,b); 
    hp.imode = i;
}

class handphone
{
    pubic handphone();
    pubic handphone(char* b, bool i);
}c+
```

**더블 디스패치**

* 두번 메시지를 전달한다는 의미

ex)가위바위보&#x20;

하나의 기본 클래스인 Hand

세개의 파생 클래스 Gawi, Bawi, Bo 가 정의

Check는 승패를 결정

Check 함수에 대한 정의는 클래스에 따라 달라짐

{% code overflow="wrap" %}
```cpp
#include <iostream.h>
#include <stdlib.h>
#include <time.h>

public class Hand
{
    public :
        virtual void Check(Hand* h) = 0;
        virtual void BawiJudge() = 0;
        virtual void GawiJudge() = 0;
        virtual void BoJudge() = 0;      
};

public class Bawi : public Hand
{
    public :
        void Check(Hand* h);
        void BawiJudge();
        void GawiJudge();
        void BoJudge();
};

    
 void Bawi::Check(Hand* h)
 {
     h->BawiJudge();
     return;
 }

void Bawi::BawiJudge
{
    cout << "비겼습니다!\n";
    return;
}

void Bawi::GawiJudge
{
    cout << "바위가 이겼습니다.!\n";
    return;
}

void Bawi::BoJudge
{
    cout << "바위가 졌습니다.!\n";
    return;
}


public class Gawi : public Hand
{
    public :
        void Check(Hand* h);
        void BawiJudge();
        void GawiJudge();
        void BoJudge();
};

void Gawi::Check(Hand* h)
 {
     h->GawiJudge();
     return;
 }

void Gawi::BawiJudge
{
    cout << "가위가 졌습니다!\n";
    return;
}

void Gawi::GawiJudge
{
    cout << "비겼습니다.!\n";
    return;
}

void Gawi::BoJudge
{
    cout << "가위가 이겼습니다.!\n";
    return;
}

public class Bo : public Hand
{
    public :
        void Check(Hand* h);
        void BawiJudge();
        void GawiJudge();
        void BoJudge();
};

void Bo::Check(Hand* h)
 {
     h->boJudge();
     return;
 }

void Bo::BawiJudge
{
    cout << "보가 이겼습니다.!\n";
    return;
}

void Bo::GawiJudge
{
    cout << "보가 졌습니다.!\n";
    return;
}

void Bo::BoJudge
{
    cout << "비겼습니다.!\n";
    return;
}

void main()
{
    Bawi ba;
    Gawi ga;
    Bo bo;

    cout << "바위 vs  가위 ..."
    ga.Check(&ba);
    //output : 
    //바위 vs  가위 ... 바위가 이겼습니다.!
}
```
{% endcode %}



ex) 바위, 가위일 경우

<pre class="language-cpp"><code class="lang-cpp">//Main
Bawi b;
Gawi g;
b.Check(&#x26;b);

//Bawi Class
void Bawi::Check(Hand* h)
{
    h->BawiJudge();
<strong>    return;
</strong>}

//Gawi Class
void Gawi::BawiJudge()
{
    cout &#x3C;&#x3C; "가위가"졌습니다.\n";
    return;
}
</code></pre>











