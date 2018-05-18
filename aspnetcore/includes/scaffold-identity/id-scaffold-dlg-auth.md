Uruchom tworzenia szkieletu tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W lewym okienku **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **dodać**.
* W **tożsamości Dodaj** okno dialogowe, wybierz odpowiednie opcje.
  * Wybierz stronę układu istniejących lub pliku układu zostaną zastąpione niepoprawny kod znaczników. Po wybraniu istniejący plik _Layout.cshtml jest **nie** zastąpione.

 Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC 
* Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do zastąpienia. Należy wybrać co najmniej jeden plik, aby dodać kontekstu danych. 
  * Wybierz klasy kontekstu danych.
  * Wybierz **dodać**.
* Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć klasę użytkownika niestandardowego dla tożsamości:
  * Wybierz **+** przycisk, aby utworzyć nowy **klasa kontekstu danych**.
  * Wybierz **dodać**.
  
Uwaga: Jeśli tworzysz nowy kontekst użytkownika, nie trzeba wybrać plik do zastąpienia.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli tworzenia szkieletu ASP.NET nie został wcześniej zainstalowany, zainstaluj go:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator --version 2.1.0-rc1-final
```

Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku. Uruchom następujące polecenie w katalogu projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji tworzenia szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu Uruchom tworzenia szkieletu tożsamości z wybranymi opcjami. Na przykład można skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalna liczba plików, uruchom następujące polecenie. Użyj poprawnej nazwy FQDN kontekst bazy danych:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```
-------------
