# LSP(Liskov Substitaion Principle)

<span style="font-familly:Papyrus;font-size:2em;">리스코프 치환 원칙</span>

<br/>

Liskov 치환 원칙은 "부모 클래스 대신 자식 클래스를 사용할 수 있어야 하며 수정없이 그것이 가능해야 한다." 다시말해 자식 클래스가 부모클래스의 동작에 영향을 미치지 않아야 한다. 즉, 자식클래스는 기본클래스를 대체 할 수 있어야 한다.

부모 클래스의 인스턴스를 사용하는 위치에 자식 클래스의 인스턴스를 대신 사용했을 때 코드가 원래 의도대로 작동해야 한다는 의미이다.
이것을 부모 클래스와 자식 클래스 사이의 행위가 일관성이 있다고 말한다.


이 원칙은 OCP 의 확장일 뿐이다. 그리고 새로운 파생클래스가 기본클래스의 동작을 변경하지 않고 확장하는지 확인해야한다.  LSP 를 위반하는 실제 사례이다.

- 아버지는 의사이며 아들은 축구 선수이다. 여기서 아들은 같은 계측에 속해 있어도 아버지를 대체 할 수 없다.


코드를 예로 들자. 우리는 SQL 파일을 사용하여 데이터를 관리하는 App를 설계해야한다. 폴더에서 SQL 파일을 불러오고 저장하는 기능이 필요하다. 따라서 Load 및 Save 하는 Class 를 만든다.

```
public class SqlFile
{
    public string FilePath {get;set;}
    public string FileText {get;setl}
    public string LoadText()
    {
        //Sql 파일에서 text를 읽는다.
    }
    public string SaveText()
    {  
        //Sql 파일으로 text를 저장한다.
    }
}

public class SqlFileManager
{
    public List<SqlFile> ListSqlFiles {get;set;}

    public string GetTextFromFiles()
    {
        StringBuilder objStrBuilder = new();
        foreach(var objFile in ListSqlFiles)
        {
            objStrBuilder.Append(objFile.LoadText());
        }
    }   

    public void SaveTextIntoFiles()
    {
        foreach(var objFile in LstSqlFiles)
        {
            objFile.SaveText();
        }
    }
}
```

좋아. 위의 코드는 괜찮아 보인다. 하지만, 몇시간 뒤 우리의 리더가 읽기전용파일이 Application 폴더에 있다고 말한다. 그래서 우리는 읽기전용 파일이 저장되는 것을 막아야한다.

SqlFileManager 에 조건을 추가하여 수정하면 될 것 같다. 우리는 ReadOnlySqlFile Class를 SqlFile을 상속받아 만든다. 그리고  SaveTextIntoFiles method 에서 읽기전용파일을 SaveText하는 것을 방지하도록 해야한다.

```
public class SqlFileManager
{
   public List<SqlFile? lstSqlFiles {get;set}
   public string GetTextFromFiles()
   {
      StringBuilder objStrBuilder = new StringBuilder();
      foreach(var objFile in lstSqlFiles)
      {
         objStrBuilder.Append(objFile.LoadText());
      }
      return objStrBuilder.ToString();
   }
   public void SaveTextIntoFiles()
   {
      foreach(var objFile in lstSqlFiles)
      {
         //Check whether the current file object is read-only or not.If yes, skip calling it's
         // SaveText() method to skip the exception.

         if(! objFile is ReadOnlySqlFile)
         objFile.SaveText();
      }
   }
}
```

위의 코드와 같이 SqlFileManager Class의 SaveTextIntoFiles method 를 변경하여 ReadOnlySqlFile이 아닌 경우 SaveText하도록 변경하였다. SqlFileManager 를 변경하지 않고서는 ReadOnlySqlFile Class는 부모 대신 사용 할 수 없다. 그래서 우리는 이 디자인은 LSP를 따르지 않고 있다고 한다. LSP를 따르도록 변경하고자 한다. 
SqlFileManager Class 를 나머지 Block 과 독립적으로 하는 Interface를 만든다.
```
public interface IReadableSqlFile
{
    string LoadText();
}
public interface IWriteableSqlFile
{
    string SaveText();
}
```

그리고 우리는 읽기전용파일에서 텍스트만 읽는 ReadOnlySqlFile class를 통해 IReadableSqlFile 를 구현합니다. SqlFile은 읽고 쓰기 둘다 가능하므로 IReadableSqlFile 와, IWriteableSqlFile 를 상속받습니다.

```
public class SqlFile: IWritableSqlFile,IReadableSqlFile
{
   public string FilePath {get;set;}
   public string FileText {get;set;}
   public string LoadText()
   {
      /* Code to read text from sql file */
   }
   public void SaveText()
   {
      /* Code to save text into sql file */
   }
}
```

그리고 SqlFileManager 는 다음과 같이 변경됩니다.

```
public class SqlFileManager
{
   public string GetTextFromFiles(List<IReadableSqlFile> aLstReadableFiles)
   {
        StringBuilder objStrBuilder = new StringBuilder();
        foreach(var objFile in aLstReadableFiles)
        {
            objStrBuilder.Append(objFile.LoadText());
        }
        return objStrBuilder.ToString();
   }
   public void SaveTextIntoFiles(List<IWritableSqlFile> aLstWritableFiles)
   {
        foreach(var objFile in aLstWritableFiles)
        {
            objFile.SaveText();
        }
   }
}
```

GetTextFromFile method 는 IReadOnlySqlFile 인터페이스를 구현하는 클래스의 인스턴스 목록만 가져옵니다. 즉, SqlFile의 ReadOnlySqlFile 클래스 인스턴스를 의미합니다. 그리고 SaveTextIntoFiles method 는 IWriteableSqlFile 인터페이스를 구현하는 클래스의 목록(이 경우 SqlFile)만 가져옵니다. 그래서 우리는 LSP를 따르고 있습니다. 그리고 우리는 ISP 도 사용하였다. 


==========================================================

[<- Go to Home](../SUMMARY.md)