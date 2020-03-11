Uruchom szkielet tożsamości:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.
* W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.
* W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.
  * Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem. Na przykład `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC
  * Wybierz przycisk **+** , aby utworzyć nową **klasę kontekstu danych**.
* Wybierz pozycję **Dodaj**.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

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

W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami. Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie:

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
