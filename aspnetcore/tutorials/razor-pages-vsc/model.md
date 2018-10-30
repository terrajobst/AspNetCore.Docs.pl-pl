---
title: Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code
author: rick-anderson
description: Dowiedz się, jak dodać model do aplikacji stron Razor w programie ASP.NET Core przy użyciu programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244726"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

* Dodaj folder o nazwie *modeli*.
* Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.
* Dodaj następujący kod do *Models/Movie.cs* pliku:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>Entity Framework Core NuGet pakiet dla oprogramowania SQLite

W wierszu polecenia Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w *Startup.cs* pliku.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu Movie

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* **Aby uzyskać Windows**: Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **Dla systemu macOS i Linux**: Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.

> [!div class="step-by-step"]
> [Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages-vsc/page)
