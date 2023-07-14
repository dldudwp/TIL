# SRP(Single Responsibility Principle)

<span style="font-familly:Papyrus;font-size:2em;">단일 책임 원칙 </span>

<br/>

"SRP는 모든 소프트웨어 모듈은 변경하는 이유가 하나여야 한다."

이 말은 코드에서 모든 클래스 또는 구조체는 하나의 일을 해야하고 하나의 목적을 가져야한다. 우리의 클래스는 맥가이버 칼처럼 둘 중 하나를 변경 수 있으면 안되고 전체 도구를 바꿔야한다. 이것은 클래스에 하나의 매서드만 있어야한다는 뜻은 아니다. 단일 책임과 관련된 많은 요소가 있을 수 있다.

Single Reponsibility Princile 은 어플리케이션 설계단계에서 클래스를 식별하는 좋은 방법이다. 그리고 클래스가 변경될 수 있는 모든방법을 생각하게 한다. 하지만 어플리케이션이 어떻게 동작하는지 큰그림이 있을 때 책임들의 좋은 분리가 가능하다.

```
public class UserService
{
   public void Register(string email, string password)
   {
      if (!ValidateEmail(email))
         throw new ValidationException("Email is not an email");
         var user = new User(email, password);

         SendEmail(new MailMessage("mysite@nowhere.com", email) { Subject="HEllo foo" });
   }
   public virtual bool ValidateEmail(string email)
   {
     return email.Contains("@");
   }
   public bool SendEmail(MailMessage message)
   {
     _smtpClient.Send(message);
   }
}
```
<br/>

- 위의 코드는 잘짜여진것처럼 보인다. 하지만 SRP 를 지키지 않았다.
SendEmail 과 VaildateEmail 메소드는 UserService에서 어떤 관련도 없다.
따라서 아래와 같이 변경

<br/>

```
public class UserService
{
   EmailService _emailService;
   DbContext _dbContext;
   public UserService(EmailService aEmailService, DbContext aDbContext)
   {
      _emailService = aEmailService;
      _dbContext = aDbContext;
   }
   public void Register(string email, string password)
   {
      if (!_emailService.ValidateEmail(email))
         throw new ValidationException("Email is not an email");
         var user = new User(email, password);
         _dbContext.Save(user);
         emailService.SendEmail(new MailMessage("myname@mydomain.com", email) {Subject="Hi. How are you!"});

      }
   }
   public class EmailService
   {
      SmtpClient _smtpClient;
   public EmailService(SmtpClient aSmtpClient)
   {
      _smtpClient = aSmtpClient;
   }
   public bool virtual ValidateEmail(string email)
   {
      return email.Contains("@");
   }
   public bool SendEmail(MailMessage message)
   {
      _smtpClient.Send(message);
   }
}
```
==========================================================

[<- Go to Home](../SUMMARY.md)