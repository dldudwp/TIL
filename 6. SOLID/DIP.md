# DIP(Dependency Inversion Principle)

<span style="font-familly:Papyrus;font-size:2em;">의존성 역전 원칙</span>

<br/>

의존성 역전원칙은 "상위모듈/클래스가 하위모듈/클래스에 의존해서는 안된다."
첫째. 둘다 추상화에 의존해야 한다. 
둘째. 추상화는 세부사항에 의존해서는 안된다.
셋째. 세부사항으 추상화에 의존해야한다.

상위모듈/클래스는 시스템에서 비즈니스 규칙 또는 논리를 구현한다. 하위모듈/클래스는 보다 자세한 작업을 처리

하위모듈/클래스는 일부 다른 클래스에 의존 및 상호작용하는 다른 클래스가 많이 있을 경우 상위모듈/클래스에 밀접하게 결합되었다고 한다. 클래스가 다른 클래스의 설계 및 구현에 대해 명시적으로 알고 있는 경우 한 클래스를 변경하면 다른 클래스가 손상될 위험이 있다. 따라서 이러한 상위 및 하위 모듈/클래스는 최대한 느슨한 결합이 필요하다. 그리고 위해서 둘다 서로 추상화에 의존하도록 만들어야한다. 다음은 예제이다.


예외 스택추적을 파일에 기록하는 오류 로깅 모듈에서 작업한다고 가정.

```
public class FileLogger
{
    public void LogMessage(string aStackTrace)
    {

    }
}

public class ExceptionLoagger
{
    public void LogIntoFile(Exception aException)
    {
        FileLogger objFileLogger = new();
        objFileLogger.LogMessage(GetUserReadableMessage(aException));
    }

    public string GetUserReadableMessage(Exception ex)
    {
        string strMessage = string.Empty;
        //..
        return strMessage;
    }
}

public class DataExporter
{
    public void ExportDataFromFile()
    {
        try{

        }
        catch(Exception ex)
        {
            new ExceptionLogger().LogIntoFile(ex);
        }
    }
}
```

좋아. 우리는 이대로 프로그램을 짰다. 그러나 클라이언트는 IO 예외가 발생하면 이 스택 추적을 데이터베이스에 저장하는 기능을 추가요청하였다. 그것도 별로 큰 일은 아니다. 여기에 Database에 기록하여는 기능을 제공하는 class를 하나 더 추가하고 스택추적을 기록하기 위해 새 클래스와 상호작용하는 ExceptionLogger에 추가하면된다.

```
public class DbLogger
{
   public void LogMessage(string aMessage)
   {
      //Code to write message in the database.
   }
}

public class FileLogger
{
   public void LogMessage(string aStackTrace)
   {
      //code to log stack trace into a file.
   }
}
public class ExceptionLogger
{
   public void LogIntoFile(Exception aException)
   {
      FileLogger objFileLogger = new FileLogger();
      objFileLogger.LogMessage(GetUserReadableMessage(aException));
   }
   public void LogIntoDataBase(Exception aException)
   {
      DbLogger objDbLogger = new DbLogger();
      objDbLogger.LogMessage(GetUserReadableMessage(aException));
   }
   private string GetUserReadableMessage(Exception ex)
   {
      string strMessage = string.Empty;
      //code to convert Exception's stack trace and message to user readable format.
      ....
      ....
      return strMessage;
   }
}
public class DataExporter
{
   public void ExportDataFromFile()
   {
      try {
         //code to export data from files to database.
      }
      catch(IOException ex)
      {
         new ExceptionLogger().LogIntoDataBase(ex);
      }
      catch(Exception ex)
      {
         new ExceptionLogger().LogIntoFile(ex);
      }
   }
}
```

좋아. 지금은 괜찮아보인다. 하지만 클라이언트가 새로운 Logging을 도입하려고 할때마다 새 method 를 추가하여 ExceptionLogger 를 변경해야한다. 
이경우 메시지를 다양한 대한에 기록하는 기능을 제공하는 많은 기능이 있는 ExceptoinLogger class 가 탄생하게 된다.
이 문제가 발생하는 이유는 무엇일까? ExceptionLogger는 하위 클래스 FileLogger 및 DBLogger에 직접 연결하여 예외를 기록하기 때문이다. 이 ExceptionLogger class가 해당 class와 느슨하게 결합 될 수 있도록 설계를 변경해야한다. 이를 위해서 ExceptionLogger가 하위클래스에 직접 의존하는 대신 예외를 기록하기 위해 추상화에 연결할 수 있도록 그들 사이에 추상화를 도입해야 한다.

```
public interface ILogger
{
    void LogMessage(string aString);
}

public class DbLogger : ILogger
{
    public void LogMessage(string aStackTrace)
    {

    }
}
public class FileLogger : ILogger
{
    public void LogMessage(string aStackTrace)
    {
        
    }
}
```

이제 ExceptionLogger class에서 DataExporter class로 하위수준 클래스로 이동하여 ExceptionLogger가 하위수준 클래스인 FileLogger 및 EventLogger와 느슨한 결합이 되도록한다. 그리고 그렇게 함으로써 발생하는 Exception 에 따라 어떤 종류의 Logger를 호출해야 하는지 결정하기 위해 DataExporter class에 프로비저닝을 제공한다.

```
public class ExceptionLogger
{
    private ILogger _logger;
    public ExceptionLogger(ILogger logger)
    {
        _logger = logger;
    }
    public void LogException(Exception exception)
    {
        string strMessage = GetUserReadableMessage(exception);
        thils._logger.LogMessage(strMessage);

    }
    private string GetUserReadableMessage(Exception exception)
    {
        string strMessage = string.Empty;
        //..
        return strMessage;
    }
}

public class DataExporter
{
    public void ExportDataFromFile()
    {
        ExceptionLogger _exceptionLogger;
        try{

        }
        catch(IOException ex)
        {
            _exceptionLogger = new ExceptionLogger(new DbLogger());
            _exceptionLogger.LogException(ex);
        }
        catch(Exception ex)
        {
            _exceptionLogger = new ExceptionLogger(new FileLogger());
            _exceptionLogger.LogException(ex);
        }
    }
}
```

하위 클래스에 대한 종속성을 성공적으로 제거 했다. 이 ExceptionLogger는 스택추적을 기록하기 위해 FileLogger 및 EventLogger 클래스에 의존하지 않는다. 새로운 로깅을 위해 더이상 ExceptionLogger으 코드를 변경할 필요가 없다.
ILogger 인터페이스를 구현하고 DataExporter 클래스의 ExportDataFromFile method 에 다른 catch 블록을 추가하면된다.

```
public class EventLogger : ILogger
{
    public void LogMessage(string aStackTrace)
    {

    }
}
``
public class DataExporter
{
   public void ExportDataFromFile()
   {
      ExceptionLogger _exceptionLogger;
      try {
         //code to export data from files to database.
      }
      catch(IOException ex)
      {
         _exceptionLogger = new ExceptionLogger(new DbLogger());
         _exceptionLogger.LogException(ex);
      }
      catch(SqlException ex)
      {
         _exceptionLogger = new ExceptionLogger(new EventLogger());
         _exceptionLogger.LogException(ex);
      }
      catch(Exception ex)
      {
         _exceptionLogger = new ExceptionLogger(new FileLogger());
         _exceptionLogger.LogException(ex);
      }
   }
}
```

좋다. 여기서 DataExporter class의 catch 블록에 종속성을 도입했다. 따라서 누군가는 작업을 완료하기 위해 필요한 개체를 ExceptionLogger에 제공할 책임이 있어야한다.

실제 예를 들면, 특정한 치수와 그 의자를 만드는데 사용되는 나무 종료가 있고 나무 의자를 갖고 싶다고 가정하자. 우리는 치수와 목제에 대한 의사결정을 목수에게 맡길 수 없다. 여기서 목수의 일은 도구로 우리의 요구사항에 따라 의자를 만드는 것이고 우리는 그에게 좋은 의자를 만들기 위한 사양을 제공한다.

그렇다면 디자인에서 얻는 이점은 무엇인가? 새로운 로깅 기능을 도입해야 할때마다 DataExporter 및 ExceptionLogger class를 수정해야한다. 그러나 업데이트된 디자인에서 새로운 예외 로깅 기능에 대한 catch 블록만 추가하면 된다. 시스템 , 요구사항, 환경을 제대로 이해하고 DIP 을 따라야 할 영역을 찾기만 하면 된다. 결합은 본질적으로 나쁜것은 아니다.. 어느정도의 커플링이 없으면 소프트웨어가 아무것도 하지 않기 때문이다.



==========================================================

[<- Go to Home](../SUMMARY.md)