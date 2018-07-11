# <a name="custom-webhost-service-sample"></a>Niestandardowe hostem sieci Web usług — przykład

Ten przykład pokazuje, jak hostowanie aplikacji ASP.NET Core jako usługę Windows bez użycia usług IIS. W tym przykładzie przedstawiono scenariusz opisany w [hostowanie aplikacji ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instrukcje

Przykładowa aplikacja jest zmodyfikować zgodnie z instrukcjami wyświetlanymi w aplikacja internetowa ze stronami Razor [hostowanie aplikacji ASP.NET Core w usłudze Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Aby uruchomić aplikację w usłudze, wykonaj następujące czynności:

1. Utwórz folder na *c:\svc*.

1. Opublikuj aplikację w folderze z `dotnet publish --configuration Release --output c:\\svc`. Polecenie przenosi zasoby aplikacji *svc* folderu, w tym wymagane `appsettings.json` pliku i `wwwroot` folderu.

1. Otwórz **administratora** wiersza polecenia.

1. Wykonaj następujące polecenia:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Odstęp między równości a początkiem ciąg ścieżki jest wymagana.*

1. W przeglądarce przejdź do `http://localhost:5000` i sprawdź, czy usługa jest uruchomiona. Aplikacja przekierowuje do bezpiecznego punktu końcowego `https://localhost:5001`.

1. Aby zatrzymać usługę, użyj polecenia:

   ```console
   sc stop MyService
   ```

Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami, szybki sposób, aby udostępnić komunikaty o błędach jest dodanie dostawcy logowania, takie jak [Dostawca dziennika zdarzeń Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Innym rozwiązaniem jest Sprawdź dziennik zdarzeń aplikacji, za pomocą Podglądu zdarzeń w systemie. Na przykład w tym miejscu jest nieobsługiwany wyjątek FileNotFound błędu w dzienniku zdarzeń aplikacji:

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
