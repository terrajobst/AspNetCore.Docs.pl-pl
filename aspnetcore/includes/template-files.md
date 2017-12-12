* Pliku Startup.CS: [Klasa początkowa](../fundamentals/startup.md) — klasa Konfiguruje potok żądań, która obsługuje wszystkie żądania kierowane do aplikacji.
* Plik program.CS: [klasy Program](../fundamentals/index.md) zawiera główny punkt wejścia aplikacji.
* firstapp.csproj: [pliku projektu](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) format pliku projektu MSBuild dla aplikacji platformy ASP.NET Core. Zawiera odwołań projektów odwołań NuGet i inne powiązane elementy projektu.
* appSettings.JSON / appsettings. Development.JSON: Ustawienia pliku konfiguracji środowisko podstawowej aplikacji. [Zobacz Konfiguracja](xref:fundamentals/configuration/index).
* bower.JSON: Bower zależności pakietów dla projektu.
* .bowerrc: plik konfiguracyjny bower, który określa, gdzie można zainstalować składniki, gdy Bower pobiera zasoby.
* bundleconfig.JSON: pliki konfiguracji tworzenie pakietów i zminimalizowania liczby frontonu zasoby JavaScript i CSS.
* Widoki: Zawiera widoki Razor. Widoki są składnikami wyświetlającymi interfejs użytkownika aplikacji (UI). Ogólnie rzecz biorąc to interfejs użytkownika wyświetla dane modelu.
* Kontrolery: Początkowo zawiera kontrolerów MVC *HomeController.cs*. Kontrolery są klasy, które obsługuje żądania przeglądarki.
* Wwwroot: folder główny aplikacji sieci Web.

Aby uzyskać więcej informacji, zobacz [wzorzec MVC](xref:mvc/overview).
