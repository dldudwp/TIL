---
description: ?��존성 주입
---

# DependencyInjection



LIB : Microsoft.Extensions.DependencyInjection



IOC

* ��⼺�� ���̴µ� ���
* �鿣�忡 ���ԵǴ� ��������(ViewModel �����ڿ� �Ű������� ����) �� ����µ� ���



���� ����

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



���⼭ ��� ���ø����̼� ���� �� ViewMOdel �� ��ϵ�


