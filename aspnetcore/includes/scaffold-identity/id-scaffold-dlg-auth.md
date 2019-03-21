---
ms.openlocfilehash: 53774177030adf8a61606a696af85cd1f57d6ab9
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320303"
---
Uruchom Generator szkieletu tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.
* W **tożsamość usługi ADD** okno dialogowe, wybierz odpowiednie opcje.
  * Wybierz istniejący stronę układu lub plik układu zostanie zastąpiony niepoprawny kod znaczników. Kiedy istniejący  *\_Layout.cshtml* plików jest zaznaczona, jest **nie** zastąpione.

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

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku. Uruchom następujące polecenie w katalogu projektu:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:

```console
dotnet aspnet-codegenerator identity -h
```

W folderze projektu należy uruchomić Generator szkieletu tożsamości z wybranymi opcjami. Na przykład aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalną liczbę plików, uruchom następujące polecenie. Użyj poprawnej nazwy FQDN kontekst bazy danych:

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

PowerShell używa średnika jako separatora polecenia. Przy użyciu programu PowerShell, ucieczki średnikami, na liście plików lub umieścić na liście plików w cudzysłów. Na przykład:

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

Jeśli Generator szkieletu tożsamości jest uruchomiony bez określenia `--files` flagi lub `--useDefaultUI` Flaga wszystkie dostępne strony tożsamości interfejsu użytkownika zostanie utworzona w projekcie.

---
