---
description: 의존성 주입
---

# DependencyInjection



설치필요i    : Microsoft.Extensions.DependencyInjection



IOC

* 모듈성을 높이는데 사용
* 백엔드에 삽입되는 여러서비스(ViewModel 생성자에 매개변수로 전달) 을 만드는데 사용



서비스 구성

```
public sealed partial class App : Application
{
    public App()
    {
        Services = ConfigureServices();
        startup += App_Startup;
    }
    
    private void App_Startup(object sender, StartupEventArgs e)
    {
        var mainView = AApp.Current.Services.GetService<MainView>();
        mainView.SHow();
    }
    
    public new static App Current => (App)Application.Current;

    public IServiceProvider Services { get; }


    private static IServiceProvider ConfigureServices()
    {
        var services = new ServiceCollection();

        //services.AddSingleton<IFilesService, FilesService>();
        //services.AddSingleton<ISettingsService, SettingsService>();
        //services.AddSingleton<IClipboardService, ClipboardService>();
        //services.AddSingleton<IShareService, ShareService>();
        //services.AddSingleton<IEmailService, EmailService>();

        return services.BuildServiceProvider();
    }
}
```



여기서 모든 어플리케이션 서비스 및 ViewMOdel 이 등록됨





