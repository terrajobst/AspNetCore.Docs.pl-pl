---
title: Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code
author: rick-anderson
description: Dowiedz się, jak dodać model do aplikacji stron Razor w programie ASP.NET Core przy użyciu programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38152974"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

* Dodaj folder o nazwie *modeli*.
* Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.
* Dodaj następujący kod do *Models/Movie.cs* pliku:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Narzędzia platformy EF dla interfejsu wiersza polecenia (CLI) znajdują się w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku. **Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.

Edytuj *RazorPagesMovie.csproj* pliku:

* Wybierz **pliku** > **Otwórz plik**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.
* Dodaj odwołanie do narzędzia dla `Microsoft.EntityFrameworkCore.Tools.DotNet` do drugiego  **\<ItemGroup >**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu Movie

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

**Uwaga: Uruchom następujące polecenie na Windows. Dla systemu MacOS i Linux zobacz następne polecenie**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* W systemach MacOS i Linux uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Zamknij program Visual Studio i ponownie uruchom polecenie.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.

> [!div class="step-by-step"]
> [Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages-vsc/page)
