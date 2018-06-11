# <a name="custom-webhost-service-sample"></a>Przykład usługi WebHost niestandardowych

W tym przykładzie pokazano, jak na potrzeby hostowania aplikacji platformy ASP.NET Core jako usługa systemu Windows bez korzystania z usług IIS. W tym przykładzie przedstawiono scenariusz opisany w [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instrukcje

Przykładowa aplikacja jest zmodyfikować zgodnie z instrukcjami wyświetlanymi w aplikacji sieci web Razor strony [hostowania aplikacji platformy ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:

1. Utwórz folder na *c:\svc*.

1. Publikowanie aplikacji w folderze `dotnet publish --configuration Release --output c:\\svc`. Polecenie przenosi zasoby aplikacji do *svc* folderu, w tym wymaganych `appsettings.json` pliku i `wwwroot` folderu.

1. Otwórz **administratora** wiersza polecenia.

1. Uruchom następujące polecenia:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Odstęp między znak równości i początku ciąg ścieżki jest wymagana.*

1. W przeglądarce przejdź do `http://localhost:5000` i sprawdź, czy usługa jest uruchomiona. Aplikacja przekierowuje do bezpiecznego punktu końcowego `https://localhost:5001`.

1. Aby zatrzymać usługę, należy użyć polecenia:

   ```console
   sc stop MyService
   ```

Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami, szybko udostępnić komunikaty o błędach jest dodanie dostawcy rejestrowania, takich jak [Dostawca dziennika zdarzeń systemu Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Inną możliwością jest wykonanie Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie. Na przykład w tym miejscu jest nieobsługiwany wyjątek błędu FileNotFound w dzienniku zdarzeń aplikacji:

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
