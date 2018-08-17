Uruchom Generator szkieletu tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.
* W **tożsamość usługi ADD** okno dialogowe, wybierz odpowiednie opcje.
  * Wybierz istniejący stronę układu lub plik układu zostanie zastąpiony niepoprawny kod znaczników. Po wybraniu istniejącego pliku _Layout.cshtml jest **nie** zastąpione.

 Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC
* Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do zastąpienia. Musisz wybrać co najmniej jeden plik, aby dodać kontekstu danych.
  * Wybierz klasy kontekstu danych.
  * Wybierz **Dodaj**.
* Aby utworzyć nowy kontekst użytkownika i potencjalnie tworzenia klasy użytkownika niestandardowego dla tożsamości:
  * Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.
  * Wybierz **Dodaj**.

Uwaga: Jeśli tworzysz nowy kontekst użytkownika, nie trzeba wybrać plik do zastąpienia.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku. Uruchom następujące polecenie w katalogu projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:

```cli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu należy uruchomić Generator szkieletu tożsamości z wybranymi opcjami. Na przykład aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalną liczbę plików, uruchom następujące polecenie. Użyj poprawnej nazwy FQDN kontekst bazy danych:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell używa średnika jako separatora polecenia. Przy użyciu programu powershell, ucieczki średnikami, na liście plików lub umieścić na liście plików w cudzysłów. Na przykład:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```
-------------
