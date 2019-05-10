* Pliku Startup.CS: [Klasa początkowa](xref:fundamentals/startup) — klasa Konfiguruje potok żądań, która obsługuje wszystkie żądania przekazywane do aplikacji.
* Program.cs : [Program klasy](xref:fundamentals/index) zawierający punkt wejścia głównego aplikacji.
* firstapp.csproj : [Plik projektu](/dotnet/articles/core/preview3/tools/csproj) formatu pliku projektu MSBuild dla aplikacji platformy ASP.NET Core. Odwołania projekt-projekt zawiera odwołania do NuGet i inne powiązane elementy projektu.
* appSettings.JSON / appsettings. Development.JSON: Plik konfiguracji ustawień aplikacji podstawowego środowiska. [Zobacz Konfiguracja](xref:fundamentals/configuration/index).
* bower.json : Zależności pakietów bower dla projektu.
* .bowerrc: Plik konfiguracji programu bower, która określa miejsce zainstalować składniki, gdy Bower pobiera zasoby.
* bundleconfig.JSON: pliki konfiguracji tworzenie pakietów i minifikacja frontonu zasoby języka JavaScript i CSS.
* Widoki: Zawiera widoki Razor. Widoki są składnikami aplikacji interfejsu użytkownika (UI). Ogólnie rzecz biorąc ten interfejs użytkownika Wyświetla określone dane modelu.
* Kontrolery: Początkowo zawiera kontrolerów MVC *HomeController.cs*. Kontrolery są w klasach, które obsługują żądania przeglądarki.
* Wwwroot: Folder główny aplikacji sieci Web.

Aby uzyskać więcej informacji, zobacz [wzorzec MVC](xref:mvc/overview).
