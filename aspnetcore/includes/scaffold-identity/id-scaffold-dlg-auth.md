::: moniker range=">= aspnetcore-3.0"

Uruchom szkielet tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.
* W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.
* W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.
  * Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem. Gdy zostanie wybrany istniejący plik *\_Layout. cshtml* , **nie** zostanie on nadpisany.

 Na przykład: `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC
* Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do przesłonięcia. Musisz wybrać co najmniej jeden plik, aby dodać kontekst danych.
  * Wybierz klasę kontekstu danych.
  * Wybierz pozycję **Dodaj**.
* Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć niestandardową klasę użytkownika dla tożsamości:
  * Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.
  * Wybierz pozycję **Dodaj**.

Uwaga: w przypadku tworzenia nowego kontekstu użytkownika nie trzeba wybierać pliku do przesłonięcia.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj wymagane odwołania pakietu NuGet do pliku projektu (\*. csproj). Uruchom następujące polecenie w katalogu projektu:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami. Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie. Użyj prawidłowej w pełni kwalifikowanej nazwy dla kontekstu bazy danych:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

Program PowerShell używa średnika jako separatora poleceń. W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnych cudzysłowach. Na przykład:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Jeśli uruchomisz szkielet tożsamości bez określenia flagi `--files` lub flagi `--useDefaultUI`, wszystkie dostępne strony interfejsu użytkownika tożsamości zostaną utworzone w projekcie.

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Uruchom szkielet tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.
* W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.
* W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.
  * Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem. Gdy zostanie wybrany istniejący plik *\_Layout. cshtml* , **nie** zostanie on nadpisany.

 Na przykład: `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC
* Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do przesłonięcia. Musisz wybrać co najmniej jeden plik, aby dodać kontekst danych.
  * Wybierz klasę kontekstu danych.
  * Wybierz pozycję **Dodaj**.
* Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć niestandardową klasę użytkownika dla tożsamości:
  * Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.
  * Wybierz pozycję **Dodaj**.

Uwaga: w przypadku tworzenia nowego kontekstu użytkownika nie trzeba wybierać pliku do przesłonięcia.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu do [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (\*. csproj). Uruchom następujące polecenie w katalogu projektu:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami. Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie. Użyj prawidłowej w pełni kwalifikowanej nazwy dla kontekstu bazy danych:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

Program PowerShell używa średnika jako separatora poleceń. W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnych cudzysłowach. Na przykład:

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Jeśli uruchomisz szkielet tożsamości bez określenia flagi `--files` lub flagi `--useDefaultUI`, wszystkie dostępne strony interfejsu użytkownika tożsamości zostaną utworzone w projekcie.

---

::: moniker-end