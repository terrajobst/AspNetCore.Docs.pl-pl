---
title: "Dodawanie modelu do aplikacji stron Razor przy użyciu programu Visual Studio dla komputerów Mac"
author: rick-anderson
description: "Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 53ac6a5a530cdf4e58908f108bcdd0baa66da934
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

* W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.
* Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik**.
* W **nowy plik** okna dialogowego:

  * Wybierz **ogólne** w okienku po lewej stronie.
  * Wybierz **pustą klasę** w słabe center.
  * Nazwa klasy **film** i wybierz **nowy**.

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Kliknij prawym przyciskiem myszy red linii o dowolnym kształcie, na przykład `MovieContext` w wierszu `services.AddDbContext<MovieContext>(options =>`. Wybierz **Quick Fix > przy użyciu RazorPagesMovie.Models;**. Dodaje programu Visual studio za pomocą instrukcji.

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

![Tworzenie strony](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Entity Framework Core NuGet pakietów dla migracji

Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Polecenie [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) łącze, aby uzyskać numer wersji do użycia. Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku. **Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.

Aby edytować *.csproj* pliku:

* Wybierz **pliku** > **Otwórz**, a następnie wybierz *.csproj* pliku.
* Wybierz **opcje**.
* Zmień **Otwórz za pomocą** do **Edytor kodu źródłowego**.

![Edytuj plik csproj](model/csproj.png)

Dodaj `Microsoft.EntityFrameworkCore.Tools.DotNet` narzędzia odwołanie do drugiego  **\<ItemGroup >**.:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

Numery wersji pokazano w poniższym kodzie były prawidłowe w czasie zapisywania.

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Dodaj pliki stron/filmów do projektu

* W programie Visual Studio, kliknij prawym przyciskiem myszy *stron* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.
* Wybierz *filmy* folderu.
* W *Chosse plików do uwzględnienia w projekcie* okno dialogowe, wybierz opcję **obejmują wszystkie**.

Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.

>[!div class="step-by-step"]
[Poprzedni: Wprowadzenie](xref:tutorials/razor-pages-mac/razor-pages-start)
[dalej: szkieletu stron Razor](xref:tutorials/razor-pages-mac/page)
