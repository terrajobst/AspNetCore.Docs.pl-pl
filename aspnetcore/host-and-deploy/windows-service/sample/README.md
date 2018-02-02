# <a name="custom-webhost-service-sample"></a>Przykład usługi WebHost niestandardowych

W tym przykładzie przedstawiono zalecany sposób hostowania aplikacji platformy ASP.NET Core w systemie Windows bez jako usługa systemu Windows za pomocą usług IIS. W tym przykładzie pokazano funkcje opisane w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instrukcje

Przykładowa aplikacja jest prostej aplikacji sieci web MVC zmodyfikować zgodnie z instrukcjami wyświetlanymi w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:

1. Utwórz folder na *c:\svc*.

1. Publikowanie aplikacji w folderze `dotnet publish --configuration Release --output c:\\svc`. Polecenie spowoduje przeniesienie zasobów aplikacji do folderu, w tym wymaganych `appsettings.json` pliku i `wwwroot` folder z zawartością.

1. Otwórz **administrator** polecenia powłoki.

1. Uruchom następujące polecenia:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. W przeglądarce przejdź do `http://localhost:5000` Aby sprawdzić, czy usługa jest uruchomiona.

1. Aby zatrzymać usługę, należy użyć polecenia:

   ```console
   sc stop MyService
   ```

Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami uruchomionej w usłudze, szybko udostępnić komunikaty o błędach jest dodanie dostawcy rejestrowania, takich jak [Dostawca dziennika zdarzeń systemu Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Inną możliwością jest wykonanie Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie. Na przykład w tym miejscu jest nieobsługiwany wyjątek błędu FileNotFound w dzienniku zdarzeń aplikacji:

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
