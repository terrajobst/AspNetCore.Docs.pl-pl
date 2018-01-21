---
title: "Dodawanie modelu do aplikacji stron Razor przy użyciu programu Visual Studio dla komputerów Mac"
author: rick-anderson
description: "Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 2a0c21ddc9ff44143ab828f8962946138a7ca0fa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

* Dodaj folder o nazwie *modele*.
* Dodaj klasę do *modele* folder o nazwie *Movie.cs*.
* Dodaj następujący kod do *Models/Movie.cs* pliku:

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku. **Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.

Edytuj *RazorPagesMovie.csproj* pliku:

* Wybierz **pliku** > **Otwórz plik**, a następnie wybierz *RazorPagesMovie.csproj* pliku.
* Dodaj odwołanie do narzędzia dla `Microsoft.EntityFrameworkCore.Tools.DotNet` drugiej  **\<ItemGroup >**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

**Uwaga: Uruchom następujące polecenie w systemie Windows. System MacOS i Linux zobacz następne polecenie**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* System MacOS i Linux uruchom następujące polecenie:

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

[!INCLUDE[model 4](../../includes/RP/model4.md)]Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.

>[!div class="step-by-step"]
[Poprzedni: Wprowadzenie](xref:tutorials/razor-pages-vsc/razor-pages-start)
[dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)
