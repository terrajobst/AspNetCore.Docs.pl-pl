---
ms.openlocfilehash: 648ed8087c64eaf80fd055fa7dd08041ab9a8752
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320302"
---
Uruchom Generator szkieletu tożsamości:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.
* W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.
* W **tożsamość usługi ADD** okno dialogowe, wybierz odpowiednie opcje.
  * Wybierz istniejący stronę układu lub plik układu zostanie zastąpiony niepoprawny kod znaczników. Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC
  * Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.
* Wybierz **Dodaj**.

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

W folderze projektu należy uruchomić Generator szkieletu tożsamości z wybranymi opcjami. Na przykład aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalną liczbę plików, uruchom następujące polecenie:

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
