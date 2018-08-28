---
title: Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code
author: rick-anderson
description: Dowiedz się, jak dodać model do aplikacji stron Razor w programie ASP.NET Core przy użyciu programu Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b891b921baf1fe6d167c7bfb8b4c5278ce9fe9f5
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055867"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="0aa9b-103">Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0aa9b-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0aa9b-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="0aa9b-104">Add a data model</span></span>

* <span data-ttu-id="0aa9b-105">Dodaj folder o nazwie *modeli*.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0aa9b-106">Dodaj klasę umożliwiającą *modeli* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="0aa9b-107">Dodaj następujący kod do *Models/Movie.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="0aa9b-108">Entity Framework Core NuGet pakiet dla oprogramowania SQLite</span><span class="sxs-lookup"><span data-stu-id="0aa9b-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="0aa9b-109">W wierszu polecenia Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="0aa9b-110">Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="0aa9b-111">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-111">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="0aa9b-112">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="0aa9b-112">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="0aa9b-113">Narzędzia platformy EF dla interfejsu wiersza polecenia (CLI) znajdują się w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="0aa9b-113">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="0aa9b-114">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-114">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="0aa9b-115">**Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-115">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="0aa9b-116">Edytuj *RazorPagesMovie.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-116">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="0aa9b-117">Wybierz **pliku** > **Otwórz plik**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-117">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="0aa9b-118">Dodaj odwołanie do narzędzia dla `Microsoft.EntityFrameworkCore.Tools.DotNet` do drugiego  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-118">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0aa9b-119">Tworzenie szkieletu modelu Movie</span><span class="sxs-lookup"><span data-stu-id="0aa9b-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="0aa9b-120">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="0aa9b-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0aa9b-121">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-121">Run the following command:</span></span>

<span data-ttu-id="0aa9b-122">**Uwaga: Uruchom następujące polecenie na Windows. Dla systemu MacOS i Linux zobacz następne polecenie**</span><span class="sxs-lookup"><span data-stu-id="0aa9b-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0aa9b-123">W systemach MacOS i Linux uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="0aa9b-124">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="0aa9b-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="0aa9b-125">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="0aa9b-126">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="0aa9b-126">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0aa9b-127">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="0aa9b-127">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
