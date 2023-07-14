# OCP(Open Close Principle)

<span style="font-familly:Papyrus;font-size:2em;">개방-폐쇄 원칙</span>

<br/>

Open / Closed 는 "소프트웨어는의 모듈 및 클래스는 확장을 위해 열려 있어야하며 수정을 위해 닫혀 있어야 한다."

<br/>
여기서" 확장을 위한 개방"은 새로운 요구사항이 생성될 때만 새로운 기능이 추가 될 수 있도록 모듈 및 클래스를 설계해야 한다는 뜻이며 "수정을 위해 닫힘"은 클래스를 이미 개발하였고 단위테스팅도 하여 확인이 끝난 클래스가 있다. 그런 클래스는 새로운 버그를 찾을 때까지 변경하여서는 안된다. 말했듯이 확장에 대해서는 열려있어야 한다; 우리는 상속을 사용 할 수 있다.

예를 들어보자.

아래는 Rectangle class이며 Height와 Wight 의 속성을 갖는다.
```
public class Rectangle{
   public double Height {get;set;}
   public double Wight {get;set; }
}
```

우리는 사각형의 총 면적을 계산하는 어플리케이션이 필요하다. 그래서 우리는 앞에서 SRP를 배웠기 때문에 Rectagle 안에 계산을 넣을 필요가 없다. 그래서 아래와 같이 면적을 구하는 Class 를 만들었다.

```
public class AreaCalculator
{
    public double TotalArea(Rectangle[] arrRectangles)
    {
        double area;
        foreach(var objRectangle in arrRectangles)
        {
            area += objRectangle.Height * objRectangle.Width;
        }

        return area;
    }
}
```
우리는 SRP 위반없이 계산하는 App를 만들수 있었고 현재까지 다른 이슈는 없었다. 그러나 사각형이 아닌 원의 면적을 구하는 기능도 확장하려고 한다면? 우리는 원의 면적을 구하는 방식은 다르기 때문에 이슈가 생길 것이다. 그것은 큰 문제가 아니다. 우리는 TotalArea method 를 변경하여 구할수 있을 것이다. loop 에서 객체 유형을 확인하고 계산하면 된다.

```
public class Rectangle{
   public double Height {get;set;}
   public double Wight {get;set; }
}
public class Circle{
   public double Radius {get;set;}
}
public class AreaCalculator
{
   public double TotalArea(object[] arrObjects)
   {
      double area = 0;
      Rectangle objRectangle;
      Circle objCircle;
      foreach(var obj in arrObjects)
      {
         if(obj is Rectangle)
         {
            area += obj.Height * obj.Width;
         }
         else
         {
            objCircle = (Circle)obj;
            area += objCircle.Radius * objCircle.Radius * Math.PI;
         }
      }
      return area;
   }
}
```

위의 코드는 Circle 면적을 구하는 코드를 추가했다. 삼각형 면적도 요청을 받아 추가할 수 있다. TotalArea method 에 있는 "if" 문 을 사용한다면 말이다. 그러나 새로운 도형이 나올때마다 우리는 TotalArea method를 수정해야한다. 그래서 AreaCalculator Class는 수정에 닫혀있지 못하다. 그렇다면 어떻게 이 상황을 피할 수 있을까? 일반적으로 우리는 구체적인 클래스 보다  인터페이스나 추상화 클래스 를 이용할 수 있을 것이다. 이러한 인터페이스들은 일단 개발되면 변할 수 있으므로 이에 의존하는 클래스는 변하지 않는 추상화에 의존 할 수 있다.
인터페이스를 구현하는 새 클래스를 만들어 기능을 추가할 수 있다. 아래는 수정된 예시이다.

```
public abstract class Shape
{
    public abstract double Area();
}

public class Rectangle : Shape
{
    public double Height {get;set;}
    public double Width  {get;set;}
    public override double Area()
    {
        return Height * Width;
    }
}

public class Circle : Shape
{
    public double Radius {get;set;}
    public override double Area()
    {
        return Radius * Radius * Math.PI;
    }
}

public class AreaCalculator
{
    public double TotalArea(Shape[] arrShapes)
    {
        double area = 0;
        foreach(var objShape in arrShapes)
        {
            area += objShape.Area();
        }
        return area;
    }
}
```

위의 코드는 SRP 와 OCP 를 따르고 있다. "Shape" 라는 추상클래스에서 파생하여 새로운 도형이 추가될 때마다 AreaCalculator 를 변경 할 필요가 없다.

==========================================================

[<- Go to Home](../SUMMARY.md)