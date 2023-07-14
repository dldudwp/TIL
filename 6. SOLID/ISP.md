# ISP(Interface Segregation Principle)

<span style="font-familly:Papyrus;font-size:2em;">인터페이스 분리 원칙</span>

<br/>

인터페이스 분리 원칙은 "클라이언트가 사용하지 않는 인터페이스를 구현하도록 강요해서는 안된다. 하나의 큰 인터페이스 대신 각각 하나의 하위 모듈을 제공하는 method 그룹을 기반으로 작고 많은 인터페이스가 선호된다."

인터페이스는 인터페이스를 구현하는 코드보다 인터페이스를 사용하는 코드와 더 밀접하게 관련되어 있어야 한다. 따라서 인터페이스의 method 는 class 가 구현하는 메서드가 아니라 클라이언트 코드에서 필요한 method 에 의해 정의됩니다. 그리므로 클라이언트가 사용하지 않는 인터페이스에 의존하도록 가요해서는 안된다.

class 와 마찬가지로 각 interface는 특정 목적/책임이 있어야한다.
개체가 해당 목적을 공유하지 않은 경우 인터페이스를 구현하도록 강요해서는 안된다.
인터페이스가 클수록 모든 구현자가 사용할 수 없는 method 가 포함될 가능성이 높아진다.
이것이 인터페이스 분리 원칙의 핵심이다.
아래는 ISP를 위반하는 예이다. TeamLead 및 Programmer 역할을 포함하는 IT회사를 위한 시스템을 구축하고자 한다. TeamLead는 커다란 작업을 작은 작업으로 나눌수 있어야하며 Programmer는 그것들을 수행할 수 있어야한다.

```
public interface ILead
{
    void CreatedSubTask();
    void AssignTask();
    void WorkdOnTask();
}

public class TeamLead : ILead
{
    void CreatedSubTask()
    {

    }
    void AssignTask()
    {

    }
    void WorkdOnTask()
    {

    }
}
```

좋다. 위의 코드는 좋아보인다. 그리나 나중에 Manager라는 직책이 추가되었을 때 TeamLead에 작업 할당 및 실제 작업은 하지 않는다. 다음과 같이 ILead 에 Manager class를 구현할 수 있을까?

```
public class Manager: ILead
{
   public void AssignTask()
   {
      //Code to assign a task.
   }
   public void CreateSubTask()
   {
      //Code to create a sub task.
   }
   public void WorkOnTask()
   {
      throw new Exception("Manager can't work on Task");
   }
}
```

Manager는 실제 작업은 하지 않는다. 동시에 Manager에게 작업을 할당할 수 없으므로 WorkOnTask method 는 Manager class에 없어야 한다. 그리고나 우리는 ILead 인터페이스에서 이 class를 구현하고 있다. 구체적인 방법은 제공해야한다. 여기서 우리는 Manager clas가 목적없이 WorkOnTask() method 를 구현하도록 강제하고 있는 것이다. 이것은 잘못된 것이다. ISP를 위반한 디자인이다.

우리는 세가지 역할을 가지고 있다.
1.관리자는 작업을 나누어 할당할 수 있다.
2. TeadLead는 작업을 나누어 할당하고 실제 작업할 수 있다.
3. 작업만할 수 있는 Programmer
이것을 분리하여야한다.

```
public interface IProgrammer
{
   void WorkOnTask();
}

public interface ILead
{
   void AssignTask();
   void CreateSubTask();
}

public class Programmer: IProgrammer
{
   public void WorkOnTask()
   {
      //code to implement to work on the Task.
   }
}
public class Manager: ILead
{
   public void AssignTask()
   {
      //Code to assign a Task
   }
   public void CreateSubTask()
   {
   //Code to create a sub taks from a task.
   }
}

public class TeamLead: IProgrammer, ILead
{
   public void AssignTask()
   {
      //Code to assign a Task
   }
   public void CreateSubTask()
   {
      //Code to create a sub task from a task.
   }
   public void WorkOnTask()
   {
      //code to implement to work on the Task.
   }
}

```

==========================================================

[<- Go to Home](../SUMMARY.md)